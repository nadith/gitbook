# Glossary

## Functions Related:

<table><thead><tr><th width="150"></th><th></th></tr></thead><tbody><tr><td>Function Declaration</td><td><p>Function header / prototype. Function without the body</p><pre class="language-c"><code class="lang-c">int testFunc(); /* must indicate the return type, function name, parameter type */
int testFunc2(int i, float j); /* must indicate the return type, function name, parameter type */
int testFunc3(int, float); /* must indicate the return type, function name, parameter type, parameter names/identifiers are optional */
</code></pre><p>During forward declaration, we can use function declarations to make the compiler aware of the function. The linker will basically look for its definition (function with the body) later during the linking stage.</p></td></tr><tr><td>Function Definition</td><td><p>Function header + body.</p><pre class="language-c"><code class="lang-c">void testFunc(int i, float j)
{
  printf("{ } curly braces indicate it's the body.");
}
</code></pre><p><mark style="color:orange;"><strong>Function Declaration vs Definition</strong></mark> </p><p>A function declaration is a statement containing a function prototype (  <strong>function name</strong>,   <strong>return type</strong>, the types of the <strong>parameters</strong> and their order). A function declaration is a function definition if the function prototype is also followed by a brace-enclosed body, which generates storage in the code space in the memory (week 02 lecture will cover the divisions of memory such as stack, heap, global storage, code space)</p></td></tr><tr><td>Function Invocation</td><td><p>Calling a function that is already declared or defined is called function invocation.</p><pre class="language-c"><code class="lang-c">void testFunc()
{
  printf("Hello World");
}

int main()
{
  testFunc(); /* function invocation */
  return 0;
}
</code></pre></td></tr></tbody></table>

## Variable Related

<table><thead><tr><th width="150"></th><th></th></tr></thead><tbody><tr><td>Variable Assignment</td><td><p>Assign a value.</p><pre class="language-c"><code class="lang-c">int x; /* variable declaration */

x = 10; /* variable initialization */

x = 20; /* variable assignment */
</code></pre></td></tr><tr><td>Variable Declaration</td><td><p>Allocating memory for a variable (note that this is different from variable initialization). When a variable is declared (allocated memory), the existing value (random garbage number/bits) of the memory location will become the value of that variable. Therefore it's important to always initialize the variables to some useful values before use.</p><pre class="language-c"><code class="lang-c">int x; /* variable declaration */
printf("%d", x); /* will print some garbage number since we have not initalized the variable to a useful value */
</code></pre></td></tr><tr><td>Variable Definition</td><td><p>variable declaration + initialization on the same line</p><pre class="language-c"><code class="lang-c">int x;
</code></pre></td></tr><tr><td>Variable Initialization</td><td><p>Assign a value to a variable for the first time</p><pre class="language-c"><code class="lang-c">int x; /* variable declaration */
x = 10; /* variable initialization */
</code></pre></td></tr></tbody></table>

## Misc

<table><thead><tr><th width="150"></th><th></th></tr></thead><tbody><tr><td>Format Specifiers</td><td><p>The format specifiers are used in C for input and output purposes. Using this concept the compiler can understand that what type of data is in a variable during taking input using the scanf() function and printing using printf() function. They act as placeholders.</p><pre class="language-c"><code class="lang-c">"%c", "%d", "%f", "%s", "%p" are some of the examples.
</code></pre></td></tr><tr><td></td><td></td></tr><tr><td></td><td></td></tr></tbody></table>
