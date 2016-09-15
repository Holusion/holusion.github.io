---
  layout: documentation
  title: Stingray
  abstract: Stingray controllable video playback engine plugins ABI SDK
---

# Plugins ABI

Stingray exposes a simple plugin ABI([?](https://fr.wikipedia.org/wiki/Application_binary_interface)). It allows  external plugins to set a playback speed. ABI is currently **UNSTABLE** and is meant to change even on **minor releases**.

# Interface

## Methods

#### getAxis

`int getAxis();`

get called each frame. return a value between -127 & 127

#### getQuit

`int getQuit();`

get called each frame. `1` if the input requests the program to exit  `0` otherwise

#### update

`void update();`

get called before `getAxis()` and `getQuit()`. Allow the module to update itself synchronized with the display.

## Sample Code

Create a base class `EventListener` :

{% highlight cpp %}
  /* EventListener.hpp */
  #ifndef EVENT_LISTENER_HPP
  #define EVENT_LISTENER_HPP

  class EventListener{

    public:
      EventListener(){}
      virtual ~EventListener(){}
      void update(){};
      virtual int getAxis() const = 0;
      virtual int getQuit() const = 0;
  };
  // the types of the class factories
  typedef EventListener* create_t();
  typedef void destroy_t(EventListener*);
  typedef void update_t(EventListener*);
  #endif
{% endhighlight %}

Derive it to create your own controller :

{% highlight cpp %}
  #ifndef  CONTROLLER_HPP
  #define  CONTROLLER_HPP
  #include "eventListener.hpp"

  class  Controller: public EventListener {
    public:
      Controller();
      ~Controller();
      void update();
      virtual int getAxis() const;
      virtual int getQuit() const;
  };

  extern "C"{
    // the class factories
    extern EventListener* create() {
     return new Controller;
    }
    extern void destroy(EventListener* p) {
     delete p;
    }
    extern void update(EventListener* p){
      ((Controller*)p)->update();
    }
  }
  #endif

{% endhighlight %}

The chunk of code at the end allow to export your class in a binary-compatible way.

Then code your methods accordingly :

{% highlight cpp %}
  #include "controller.hpp"

  Controller::Controller() {
    //do any init you need
  }

  Controller::~Controller() {
    //Do any cleanup you like
  }
  int Controller::getQuit() const{ //return 1 to exit Stingray}
  int Controller::getAxis() const{ //return speed axis}

  void Controller::update(){
    //Do something on each frame
  }

{% endhighlight %}

and you're done. Compile it as a `mymodule.so` file and put it in Stingray's modules directory.

# Internals

Works with `ltdl`. Useful resources are hard to find :

- Minimal example in [autobook](https://sourceware.org/autobook/autobook/autobook_98.html)
- Automake / Autoconf setup in [libtool manual](https://www.gnu.org/software/libtool/manual/html_node/Distributing-libltdl.html#Distributing-libltdl)
- dlopen in c++ [HOWTO](http://www.tldp.org/HOWTO/pdf/C++-dlopen.pdf) pages 6 and more.
