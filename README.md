# HTML Functional Markup Language (HFML): A Comprehensive Specification

**Version 1.0 - Technical Specification**

## 1. Introduction

This document presents the formal specification for HTML Functional Markup Language (HFML), a unified programming language designed to replace JavaScript in web development paradigms. HFML extends the declarative nature of HTML with powerful functional programming capabilities, creating a single coherent language that handles structure, presentation, and behavior.

## 2. Design Philosophy

HFML is built on the following core principles:

1. **Declarative First**: Programming constructs follow HTML's declarative model
2. **Semantic Clarity**: Tags clearly communicate their purpose and function
3. **Functional Paradigm**: Emphasis on immutability, pure functions, and expression-based programming
4. **DOM Integration**: The Document Object Model serves as the runtime's primary data structure
5. **Progressive Complexity**: Simple concepts expressed simply, complex capabilities available when needed

## 3. Lexical Structure

HFML follows XML syntax rules with the following key characteristics:

* Case-sensitive tags and attributes
* Must conform to well-formed XML standards
* Whitespace is generally insignificant except in text content
* Comments follow HTML syntax: `<!-- comment -->`

## 4. Formal Grammar (Extended Backus-Naur Form)

```ebnf
(* Top Level *)
Program ::= Element | Program Element

(* Basic Elements *)
Element ::= '<' Identifier Attributes? '/>' | '<' Identifier Attributes? '>' Content '</' Identifier '>'
Content ::= (Element | TextContent)*
TextContent ::= Characters
Attributes ::= (Attribute)+
Attribute ::= Identifier '=' StringLiteral

(* Identifiers and Literals *)
Identifier ::= Alpha (AlphaNum | '-' | '_')*
Alpha ::= 'a'..'z' | 'A'..'Z'
AlphaNum ::= Alpha | '0'..'9'
StringLiteral ::= '"' StringCharacters? '"' | "'" StringCharacters? "'"
StringCharacters ::= StringCharacter+
StringCharacter ::= AnyCharacterExcept('"' | "'") | EscapeSequence
EscapeSequence ::= '\' ('\\' | 'n' | 'r' | 't' | '"' | "'" | '{' | '}')
NumberLiteral ::= Integer | Float
Integer ::= '0' | ['-'] NonZeroDigit Digit*
Float ::= Integer '.' Digit+
Digit ::= '0'..'9'
NonZeroDigit ::= '1'..'9'
BooleanLiteral ::= 'true' | 'false'

(* Expressions *)
Expression ::= Literal | Variable | FunctionCall | Operation | CompoundExpression
Literal ::= StringLiteral | NumberLiteral | BooleanLiteral | NullLiteral | ArrayLiteral | ObjectLiteral
NullLiteral ::= 'null'
Variable ::= Identifier
FunctionCall ::= Identifier '(' ArgumentList? ')'
ArgumentList ::= Expression (',' Expression)*
Operation ::= Expression Operator Expression
Operator ::= '+' | '-' | '*' | '/' | '%' | '==' | '!=' | '>' | '<' | '>=' | '<=' | '&&' | '||'
CompoundExpression ::= '{' Expression '}'

(* Variables *)
VarDeclaration ::= '<var ' 'name=' StringLiteral ' ' ('value=' StringLiteral | ValueElement) '/>'
ValueElement ::= '>' Expression '</var>'

(* Data Structures *)
ArrayDeclaration ::= '<array ' 'name=' StringLiteral '>' ArrayItems '</array>'
ArrayItems ::= '<item>' Expression '</item>'*
ObjectDeclaration ::= '<object ' 'name=' StringLiteral '>' PropertyList '</object>'
PropertyList ::= ('<property ' 'name=' StringLiteral PropertyValue '/>')*
PropertyValue ::= '>' Expression '</property>' | 'value=' StringLiteral

(* Functions *)
FunctionDeclaration ::= '<function ' 'name=' StringLiteral '>' Parameters FunctionBody '</function>'
Parameters ::= ('<param ' 'name=' StringLiteral '/>') *
FunctionBody ::= '<body>' Statement* ReturnStatement? '</body>'
ReturnStatement ::= '<return>' Expression '</return>'
LambdaExpression ::= '<lambda ' 'params=' StringLiteral '>' Statement* '</lambda>'

(* Control Flow *)
IfStatement ::= '<if ' 'condition=' StringLiteral '>' ThenClause ElseClause? '</if>'
ThenClause ::= '<then>' Statement* '</then>'
ElseClause ::= '<else>' Statement* '</else>'
ForLoop ::= '<for ' 'each=' StringLiteral ' ' 'in=' StringLiteral '>' Statement* '</for>'
WhileLoop ::= '<while ' 'condition=' StringLiteral '>' Statement* '</while>'
SwitchStatement ::= '<switch ' 'value=' StringLiteral '>' CaseStatement+ DefaultCase? '</switch>'
CaseStatement ::= '<case ' 'value=' StringLiteral '>' Statement* '</case>'
DefaultCase ::= '<default>' Statement* '</default>'

(* Events *)
EventHandler ::= '<on ' 'event=' StringLiteral '>' Statement* '</on>'

(* Async Operations *)
AsyncBlock ::= '<async ' ('name=' StringLiteral)? '>' Statement* '</async>'
PromiseHandling ::= '<then>' Statement* '</then>' '<catch>' Statement* '</catch>' '<finally>' Statement* '</finally>'

(* State Management *)
StateDeclaration ::= '<state ' 'name=' StringLiteral ' ' 'initial=' StringLiteral '/>'
StateModifier ::= '<set ' 'target=' StringLiteral ' ' 'value=' StringLiteral '/>'

(* Component Definition *)
ComponentDefinition ::= '<component ' 'name=' StringLiteral '>' PropDefinitions ComponentBody '</component>'
PropDefinitions ::= ('<prop ' 'name=' StringLiteral ' ' ('type=' StringLiteral)? ' ' ('default=' StringLiteral)? '/>')*
ComponentBody ::= TemplateBlock ScriptBlock?
TemplateBlock ::= '<template>' Element* '</template>'
ScriptBlock ::= '<script>' Statement* '</script>'

(* General Statement *)
Statement ::= Element | Expression
```

## 5. Core Language Features

### 5.1 Variables and Constants

Variables are declared using the `<var>` tag. Constants use the `<const>` tag.

```html
<var name="counter" value="0" />
<const name="API_URL" value="https://example.com/api" />
```

Variable reassignment:

```html
<set target="counter" value="counter + 1" />
```

### 5.2 Data Types

HFML supports the following primitive types:

* String: `<var name="greeting" value="Hello world" />`
* Number: `<var name="count" value="42" />`
* Boolean: `<var name="isActive" value="true" />`
* Null: `<var name="empty" value="null" />`
* Undefined: (absence of a value)

And the following complex types:

#### Arrays:

```html
<array name="fibonacci">
  <item>1</item>
  <item>1</item>
  <item>2</item>
  <item>3</item>
  <item>5</item>
</array>
```

Alternatively:

```html
<var name="primes">
  <array>
    <item>2</item>
    <item>3</item>
    <item>5</item>
    <item>7</item>
  </array>
</var>
```

#### Objects:

```html
<object name="user">
  <property name="id" value="1001" />
  <property name="name">John Smith</property>
  <property name="active" value="true" />
  <property name="roles">
    <array>
      <item>editor</item>
      <item>reviewer</item>
    </array>
  </property>
</object>
```

### 5.3 Functions

#### Function Declaration:

```html
<function name="calculateTotal">
  <param name="prices" />
  <param name="taxRate" default="0.07" />
  <body>
    <var name="subtotal">
      <reduce collection="prices" initial="0">
        <lambda params="total, price">
          <math>total + price</math>
        </lambda>
      </reduce>
    </var>
    <return>
      <math>subtotal * (1 + taxRate)</math>
    </return>
  </body>
</function>
```

#### Function Invocation:

```html
<var name="total">
  <call function="calculateTotal">
    <arg>productPrices</arg>
    <arg>0.08</arg>
  </call>
</var>
```

#### Anonymous Functions (Lambdas):

```html
<var name="double">
  <lambda params="x">
    <math>x * 2</math>
  </lambda>
</var>
```

### 5.4 Control Flow

#### Conditional Statements:

```html
<if condition="user.authenticated">
  <then>
    <div class="welcome">Welcome back, <text value="user.name" />!</div>
  </then>
  <else>
    <if condition="user.registered">
      <then>
        <div class="login-prompt">Please log in to continue</div>
      </then>
      <else>
        <div class="register-prompt">Create an account to get started</div>
      </else>
    </if>
  </else>
</if>
```

#### Loops:

```html
<for each="item" in="items">
  <li class="item">
    <text value="item.name" />
    <span class="price">$<text value="item.price.toFixed(2)" /></span>
  </li>
</for>

<while condition="attempts < maxAttempts">
  <call function="attemptConnection" />
  <set target="attempts" value="attempts + 1" />
</while>
```

#### Switch Statements:

```html
<switch value="status">
  <case value="pending">
    <div class="status-indicator pending">Processing...</div>
  </case>
  <case value="approved">
    <div class="status-indicator approved">Approved</div>
  </case>
  <case value="rejected">
    <div class="status-indicator rejected">Declined</div>
  </case>
  <default>
    <div class="status-indicator unknown">Unknown Status</div>
  </default>
</switch>
```

### 5.5 Functional Programming Constructs

#### Map, Filter, Reduce:

```html
<!-- Map operation -->
<var name="doubledValues">
  <map collection="numbers">
    <lambda params="num">
      <math>num * 2</math>
    </lambda>
  </map>
</var>

<!-- Filter operation -->
<var name="activeUsers">
  <filter collection="users">
    <lambda params="user">
      <compare>user.active == true</compare>
    </lambda>
  </filter>
</var>

<!-- Reduce operation -->
<var name="total">
  <reduce collection="orderItems" initial="0">
    <lambda params="sum, item">
      <math>sum + item.price * item.quantity</math>
    </lambda>
  </reduce>
</var>
```

#### Comprehensions:

```html
<var name="squaresOfEvens">
  <comprehension>
    <expression><math>x * x</math></expression>
    <for name="x" in="numbers" />
    <where><compare>x % 2 == 0</compare></where>
  </comprehension>
</var>
```

### 5.6 Asynchronous Programming

#### Promises:

```html
<async name="fetchUserData">
  <fetch url="/api/users/{userId}">
    <then>
      <lambda params="response">
        <return><call function="response.json" /></return>
      </lambda>
    </then>
    <catch>
      <lambda params="error">
        <log level="error" message="Failed to fetch user data" />
        <return><object><property name="error" value="true" /></object></return>
      </lambda>
    </catch>
  </fetch>
</async>
```

#### Async/Await:

```html
<async name="loadUserProfile">
  <try>
    <var name="userData">
      <await>
        <call function="fetchUserData">
          <arg><access path="session.userId" /></arg>
        </call>
      </await>
    </var>
    <var name="userPreferences">
      <await>
        <call function="fetchUserPreferences">
          <arg><access path="userData.id" /></arg>
        </call>
      </await>
    </var>
    <set target="userProfileData" value="{ ...userData, preferences: userPreferences }" />
    <update target="profileView" />
  </try>
  <catch error="e">
    <set target="errorMessage" value="e.message" />
    <update target="errorNotification" />
  </catch>
</async>
```

### 5.7 Event Handling

```html
<button class="increment-button">
  Increment Counter
  <on event="click">
    <set target="counter" value="counter + 1" />
    <update target="counterDisplay" />
  </on>
</button>

<input type="text" name="username">
  <on event="input">
    <lambda params="event">
      <set target="formData.username" value="event.target.value" />
      <call function="validateUsername">
        <arg><access path="formData.username" /></arg>
      </call>
    </lambda>
  </on>
</input>
```

### 5.8 Component System

```html
<component name="UserCard">
  <prop name="user" required="true" />
  <prop name="showDetails" default="false" />
  
  <template>
    <div class="user-card">
      <h3><text value="user.name" /></h3>
      <if condition="showDetails">
        <then>
          <div class="user-details">
            <p>Email: <text value="user.email" /></p>
            <p>Joined: <text value="formatDate(user.joinDate)" /></p>
          </div>
        </then>
      </if>
      <button>
        <if condition="showDetails">
          <then>Hide Details</then>
          <else>Show Details</else>
        </if>
        <on event="click">
          <set target="showDetails" value="!showDetails" />
        </on>
      </button>
    </div>
  </template>
  
  <script>
    <function name="formatDate">
      <param name="dateString" />
      <body>
        <var name="date">
          <new constructor="Date">
            <arg>dateString</arg>
          </new>
        </var>
        <return>
          <call function="date.toLocaleDateString" />
        </return>
      </body>
    </function>
  </script>
</component>
```

Usage:

```html
<UserCard user="currentUser" showDetails="true" />
```

## 6. Standard Library

HFML includes a comprehensive standard library:

### 6.1 DOM Manipulation

```html
<append target="parentElement">
  <div class="new-element">Added content</div>
</append>

<remove target="elementToRemove" />

<replace target="oldElement">
  <div class="replacement">New content</div>
</replace>

<update target="elementToUpdate" />
```

### 6.2 String Operations

```html
<var name="greeting">
  <string:concat>
    <arg>Hello, </arg>
    <arg><access path="user.name" /></arg>
    <arg>!</arg>
  </string:concat>
</var>

<var name="firstPart">
  <string:substring text="Hello world" start="0" end="5" />
</var>
```

### 6.3 Array Operations

```html
<var name="combined">
  <array:concat>
    <arg>firstArray</arg>
    <arg>secondArray</arg>
  </array:concat>
</var>

<var name="sliced">
  <array:slice array="numbers" start="2" end="5" />
</var>
```

### 6.4 Math Functions

```html
<var name="rounded">
  <math:round><access path="value" /></math:round>
</var>

<var name="randomValue">
  <math:random min="1" max="100" />
</var>
```

### 6.5 Date and Time

```html
<var name="now">
  <date:now />
</var>

<var name="formattedDate">
  <date:format date="user.joinDate" format="YYYY-MM-DD" />
</var>
```

### 6.6 HTTP Requests

```html
<fetch url="/api/data" method="POST">
  <headers>
    <header name="Content-Type" value="application/json" />
    <header name="Authorization" value="Bearer {token}" />
  </headers>
  <body>
    <json>
      <object>
        <property name="name" value="formData.name" />
        <property name="email" value="formData.email" />
      </object>
    </json>
  </body>
  <then>
    <lambda params="response">
      <!-- Handle response -->
    </lambda>
  </then>
</fetch>
```

## 7. Runtime Model

HFML integrates with the existing DOM as its runtime environment. The HFML interpreter walks the document tree and transforms HFML constructs into their runtime representations.

### 7.1 Execution Model

1. Parse HTML tree into HFML document object
2. Initialize global scope and environment
3. Recursively process elements, evaluating expressions
4. Build reactive data dependencies
5. Establish event handlers
6. Update DOM based on initial state

### 7.2 Reactivity System

HFML implements a fine-grained reactivity system:

```html
<state name="count" initial="0" />

<div>
  Current count: <text bind="count" />
</div>

<button>
  Increment
  <on event="click">
    <set target="count" value="count + 1" />
  </on>
</button>
```

When a state value changes, all elements that depend on it are automatically updated.

## 8. Security Model

HFML includes robust security features:

### 8.1 Content Security

Content is sanitized by default, with explicit opt-in for raw HTML:

```html
<text value="userInput" /> <!-- Automatically sanitized -->
<html content="trustedHTML" /> <!-- Explicitly rendered as HTML -->
```

### 8.2 XSS Prevention

Expression evaluation occurs in a sandboxed context, preventing access to sensitive APIs.

### 8.3 Permissions Model

Access to sensitive APIs requires explicit declarations:

```html
<requires permission="geolocation" />
<requires permission="notifications" />
```

## 9. Optimization Features

HFML includes built-in performance optimizations:

### 9.1 Static Analysis

The compiler can detect and optimize:
- Dead code elimination
- Constant folding
- Tree shaking

### 9.2 Compilation Targets

HFML can be compiled to various targets:
- Optimized JavaScript
- WebAssembly (for performance-critical sections)
- Native code (for server-side execution)

### 9.3 Incremental Rendering

```html
<for each="item" in="largeDataset" incremental="true" chunk-size="50">
  <div class="item">
    <text value="item.name" />
  </div>
</for>
```

## 10. Compatibility and Migration

### 10.1 JavaScript Interoperability

```html
<script:import module="utils" from="./utils.js" />

<script:eval>
  // Raw JavaScript if needed
  const result = complexOperation(data);
  return result;
</script:eval>
```

### 10.2 Legacy HTML Support

Standard HTML elements work as expected within HFML:

```html
<div class="container">
  <h1>This is standard HTML</h1>
  <p>Mixed with <text value="dynamicContent" /></p>
</div>
```

## 11. Implementation Considerations

A conforming HFML implementation must:

1. Parse and validate HFML syntax
2. Implement the complete standard library
3. Support all control flow and expression types
4. Provide debugging and development tools
5. Ensure proper error handling and reporting
6. Maintain backward compatibility with standard HTML
7. Offer performance optimizations

## 12. Conclusion

HFML represents a unified approach to web development, combining structure, styling, and behavior into a coherent language model. By extending HTML's declarative paradigm with functional programming capabilities, HFML addresses the fragmentation and complexity issues that have historically challenged web development.

This specification defines a complete alternative to JavaScript that could potentially revolutionize web development by simplifying the mental model for developers while providing powerful capabilities for building modern, reactive web applications.

---

© 2023 HFML Language Specification Committee
