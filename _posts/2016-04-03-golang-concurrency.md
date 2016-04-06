## Concurrency

*In Go only one routine has access to the value at any given time*, so data races cannot occur by design.

Concurrent programming in many environments is made difficult by the subtleties required to implement correct access to shared variables. Go encourauges a different approach in which shared values are passed around on channels and infact, never actively shared by separate threads of execution. 

Let see Go Routines and Channels in depth:

A Go Routine is a lightweight thread that share the same address space with many others go routines, so access to shared memory must be synchronized.

A Channel is a typed conduit through which you can send and receive values with the channel operator `<-`

{% highlight golang %}
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and assign value to v.
{% endhighlight %}

As you can tell the data flows in the direction of the arrow. Please note that channels must be created before use.

By default, sends and receives block wait until the other side is ready. This allow goroutines be synchronize without explicit locks or conditions variables.

Also channels can be buffered. 

{% highlight golang %}
ch := make(chan int, 100)
{% endhighlight %}

In the last example, we defined a channel `ch` with a buffer length of 100, provided as a second argument. *Sends to a buffered channel block only when the buffer is full. Receives blocks when the buffer is empty*

In the following snippet, we are creating a channel with a two buffer length, and then we are sending two values into the channel. 

{% highlight golang %}
package main

import "fmt"

func main() {
  c := make(chan int, 2)
  c <- 1
  c <- 2
  fmt.Println(<-c)
  fmt.Println(<-c)
}
{% endhighlight %}

But if you we add more values than the buffer length, something else happen:

{% highlight golang %}
package main

import "fmt"

func main() {
  c := make(chan int, 2)
  c <- 1
  c <- 2
  c <- 3 // #somepeoplewanttowatchtheworldburn
  fmt.Println(<-c)
  fmt.Println(<-c)
  fmt.Println(<-c)
}
{% endhighlight %}

You'll get a deadlock

{% highlight golang %}
fatal error: all goroutines are asleep - deadlock!
{% endhighlight %}

That’s because we overfilled the buffer without letting the code a chance to read/remove a value from the channel. There's an easy way to fix the former deadlock by using a goroutine:

{% highlight golang %}
package main

import "fmt"

func main() {
  c := make(chan int, 2)
  c <- 1
  c <- 2
  c3 := func() { c <- 3 }
  go c3()
  fmt.Println(<-c)
  fmt.Println(<-c)
  fmt.Println(<-c)
}
{% endhighlight %}

The reason why the previous code doesn't fail is that we are adding an extra value from inside a go routine, so the following is happening:

- Our code doesn't block the main thread
- The goroutine is being called before the channel is emptied, but that's ok, since the go routine will wait until the channel is available
- When first value is read from the channel, a spot is released and our goroutine can push its value to the channel :smiley_cat: 



