---
layout: default
title: Builder
nav_order: 1
parent: Creational
grand_parent: Design_pattern
---

如果您有個物件必須建立，物件是由個別組件（Component）組合而成，個別組件建立非常複雜，但說明如何運用組件建立非常簡單，您希望將建立複雜組件與運用組件方式分離，則可以使用Builder模式。

乍看之下，Builder模式與 Abstract Factory 模式 很類似，其中最主要的差別在於，Abstract Factory模式著重在不同的工廠實作提供不同的一組產品給組件使用，產品之間並不見得有「部份」（Part of）的概念。Builder模式則強調Builder中 所建立的組件，彼此之間有著「部份」（Part of）的概念，並依Director的流程來建立組件與組件之間的關係，也就是說，Builder 組件建立與Director流程指導之間為彼此合作關 係（為強調出兩者關係，或許取名叫Director-Builder模式會更適合）。


![](https://openhome.cc/Gossip/DesignPattern/images/Builder-1.jpg)

### Builder
``` C#
namespace DotNetDesignPatternDemos.Creational.Builder
{
  class HtmlElement
  {
    public string Name, Text;
    public List<HtmlElement> Elements = new List<HtmlElement>();
    private const int indentSize = 2;

    public HtmlElement()
    {
      
    }

    public HtmlElement(string name, string text)
    {
      Name = name;
      Text = text;
    }

    private string ToStringImpl(int indent)
    {
      var sb = new StringBuilder();
      var i = new string(' ', indentSize * indent);
      sb.Append(string.Format("{i},<{Name}>\n"));
      if (!string.IsNullOrWhiteSpace(Text))
      {
        sb.Append(new string(' ', indentSize * (indent + 1)));
        sb.Append(Text);
        sb.Append("\n");
      }

      foreach (var e in Elements)
        sb.Append(e.ToStringImpl(indent + 1));

      sb.Append(string.Format("{i},<{Name}>\n"));
      return sb.ToString();
    }

    public override string ToString()
    {
      return ToStringImpl(0);
    }
  }

  class HtmlBuilder
  {
    private readonly string rootName;

    public HtmlBuilder(string rootName)
    {
      this.rootName = rootName;
      root.Name = rootName;
    }

    // not fluent
    public void AddChild(string childName, string childText)
    {
      var e = new HtmlElement(childName, childText);
      root.Elements.Add(e);
    }

    public HtmlBuilder AddChildFluent(string childName, string childText)
    {
      var e = new HtmlElement(childName, childText);
      root.Elements.Add(e);
      return this;
    }

    public override string ToString()
    {
      return root.ToString();
    }

    public void Clear()
    {
      root = new HtmlElement{Name = rootName};
    }

    HtmlElement root = new HtmlElement();
  }

  public class Demo
  {
    static void Main(string[] args)
    {
      // if you want to build a simple HTML paragraph using StringBuilder
      var hello = "hello";
      var sb = new StringBuilder();
      sb.Append("<p>");
      sb.Append(hello);
      sb.Append("</p>");
      Console.WriteLine(sb);

      // now I want an HTML list with 2 words in it
      var words = new[] {"hello", "world"};
      sb.Clear();
      sb.Append("<ul>");
      foreach (var word in words)
      {
        sb.AppendFormat("<li>{0}</li>", word);
      }
      sb.Append("</ul>");
      Console.WriteLine(sb);

      // ordinary non-fluent builder
      var builder = new HtmlBuilder("ul");
      builder.AddChild("li", "hello");
      builder.AddChild("li", "world");
      Console.WriteLine(builder.ToString());

      // fluent builder
      sb.Clear();
      builder.Clear(); // disengage builder from the object it's building, then...
      builder.AddChildFluent("li", "hello").AddChildFluent("li", "world");
      Console.WriteLine(builder);
    }
  }
}
```
### Fluent Builder 
``` c#
namespace DotNetDesignPatternDemos.Creational.Builder
{
  public class Person
  {
    public string Name;

    public string Position;

    class Builder : PersonInfoBuilder<Builder> { /* degenerate */ }

    public override string ToString()
    {
      return $"{nameof(Name)}: {Name}, {nameof(Position)}: {Position}";
    }
  }

  abstract class PersonBuilder<SELF>
    where SELF : PersonBuilder<SELF>
  {
    protected Person person = new Person();

    public Person Build()
    {
      return person;
    }
  }

  class PersonInfoBuilder<SELF> : PersonBuilder<PersonInfoBuilder<SELF>>
    where SELF : PersonInfoBuilder<SELF>
  {
    public SELF Called(string name)
    {
      person.Name = name;
      return (SELF) this;
    }
  }

  class PersonJobBuilder<SELF> : PersonInfoBuilder<PersonJobBuilder<SELF>>
    where SELF : PersonJobBuilder<SELF>
  {
    public SELF WorksAsA(string position)
    {
      person.Position = position;
      return (SELF) this;
    }
  }

  public class BuilderInheritanceDemo
  {
    static void Main(string[] args)
    {
      var builder = new Builder();
      var person = builder.WorksAsA("d").
      Console.WriteLine(person);
    }
  }
}
```
### Faceted Builder
``` c#
namespace DotNetDesignPatternDemos.Creational.BuilderFacets
{
  public class Person
  {
    // address
    public string StreetAddress, Postcode, City;

    // employment
    public string CompanyName, Position;

    public int AnnualIncome;

    public override string ToString()
    {
      return $"{nameof(StreetAddress)}: {StreetAddress}, {nameof(Postcode)}: {Postcode}, {nameof(City)}: {City}, {nameof(CompanyName)}: {CompanyName}, {nameof(Position)}: {Position}, {nameof(AnnualIncome)}: {AnnualIncome}";
    }
  }

  public class PersonBuilder // facade 
  {
    // the object we're going to build
    protected Person person = new Person(); // this is a reference!

    public PersonAddressBuilder Lives => new PersonAddressBuilder(person);
    public PersonJobBuilder Works => new PersonJobBuilder(person);

    public static implicit operator Person(PersonBuilder pb)
    {
      return pb.person;
    }
  }

  public class PersonJobBuilder : PersonBuilder
  {
    public PersonJobBuilder(Person person)
    {
      this.person = person;
    }

    public PersonJobBuilder At(string companyName)
    {
      person.CompanyName = companyName;
      return this;
    }

    public PersonJobBuilder AsA(string position)
    {
      person.Position = position;
      return this;
    }

    public PersonJobBuilder Earning(int annualIncome)
    {
      person.AnnualIncome = annualIncome;
      return this;
    }
  }

  public class PersonAddressBuilder : PersonBuilder
  {
    // might not work with a value type!
    public PersonAddressBuilder(Person person)
    {
      this.person = person;
    }

    public PersonAddressBuilder At(string streetAddress)
    {
      person.StreetAddress = streetAddress;
      return this;
    }

    public PersonAddressBuilder WithPostcode(string postcode)
    {
      person.Postcode = postcode;
      return this;
    }

    public PersonAddressBuilder In(string city)
    {
      person.City = city;
      return this;
    }
    
  }

  public class Demo
  {
    static void Main(string[] args)
    {
      var pb = new PersonBuilder();
      Person person = pb
        .Lives
          .At("123 London Road")
          .In("London")
          .WithPostcode("SW12BC")
        .Works
          .At("Fabrikam")
          .AsA("Engineer")
          .Earning(123000);

      WriteLine(person);
    }
  }
}

```
