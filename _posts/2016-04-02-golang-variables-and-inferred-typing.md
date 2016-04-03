In Golang the `var` statement declared a list of variables with the type declared last.


{% highlight golang %}
var(
  name string
  age int
  location string
)
{% end %}

Or even

{% highlight golang %}
var(
  name, location string
  age int
)
{% end %}

Variables can also be declared one by one

{% highlight golang %}
var name string
var age int
var location string
{% end %}

A var declaration can include initializers, one per variable

{% highlight golang %}
var(
  name string = "Mayra"
	age int = 27
	location string = "MTY"
)
{% end %}

If an initializer is present the type can be omitted, the variable will take the type of the initializer (*inferred typing*)

{% highlight golang %}
var(
	name = "Mayra"
	age = 27
	location = "MTY"
)
{% end %}

You can also initialize variables on the same line:

{% highlight golang %}
var(
  name, location, age = "Mayra", 27, "MTY"
)
{% end %}

Inside a function, the `:=` short assignment statement can be used in place of var declaration with implicit type

{% highlight golang %}
func main(){
	name, location := "Mayra", "MTY"
	age := 32
	fmt.Printf("%s (%d) of %s", name, age, location)
{% end %}

