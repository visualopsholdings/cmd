
# Using I2C to control an Arduino

## Context

Read the readme in: https://github.com/visualopsholdings/ringbuffer

But how about more complicated things like string data. The thing is that you can can
happily send multiple bytes to the device but you need to tell it, that's it I'm finished
NOW go and look at that thing.

This is what the Cmd class is for!

### The command (Cmd) getter

Cmd consumed the data in the buffer and waits till it finds a ;, tab, newline or space
and THEN you can get the data out and use it.

So you can use it like this:

```
RingBuffer buffer;
Cmd cmd;

void setup() {
  pinMode(7, OUTPUT);
  digitalWrite(7, LOW);
  Wire.begin(8);
  Wire.onReceive(receiveEvent);
}
void receiveEvent(int howMany) {
  while (Wire.available()) {
    buffer.write(Wire.read());
  }
}
void loop() {
  cmd.accept(&buffer);
  if (cmd.ready()) {
    char c[20];
    cmd.read(c, sizeof(c));
    if (strcmp(cmd) == "ON") {
      digitalWrite(7, HIGH);
    }
    else if (strcmp(cmd) == "OFF") {
      digitalWrite(7, LOW);
    }
    else {
      // flash the LED we got an error.
    }
  }
}
```
  
## Using the examples

Each example is named appropriately for what it does, and to get them all to build, put this
whole i2c folder into the "libraries" folder in your Arduion Sketch Directory. You can find
this in Preferences, or Settings on the macintosh.

## Development

The development process for all of this code used a normal Linux environment with the BOOST
libraries and a C++ compiler to write and run unit tests to test all of the various
things and then use that code in the Arduino IDE to flash the Arduino.

It's written in a subset of C++ that happily builds on an Arduino:

  - No dynamic memory (no new, malloc etc. Everything just static)
  - No inheritence (see above)
  - No STL (see above)

BUT for the actual development, including a fairly complex debugger for the LOGO stuff,
we use all of these things and just don't build that bit for the Arduino.

Firstly, checkout the repo:

https://github.com/visualopsholdings/ringbuffer

To be a sibling of this repo. So they need to be in the same folder like this:

- folder
  -- cmd
  -- ringbuffer

So on your Linux (or mac using Homebrew etc), get all you need:

```
$ sudo apt-get -y install g++ gcc make cmake boost
```

And then inside the "test" folder on your machine, run:

```
$ mkdir build
$ cd build
$ cmake ..
$ make
$ make test
```

And then you can go in and fiddle and change the code in lgtestsketch.cpp to change what
your sketch might look like on in your .ino and then do

```
$ make && ./CMDTest
```

And it will just run your test in your Dev environment! Just stub out all your builtin words
like "ledOn()" etc to output a string.

For an M1 or M2 mac, use this command for cmake

```
$ cmake -DCMAKE_OSX_ARCHITECTURES="arm64" ..
```

To turn on all the debugging for the various things, each header has something like:

```
//#define CMD_DEBUG
```

That you can comment out BUT while it's commented out the code won't build on the Arduino.

This stuff is just for the development environment.
  
## Change Log

24 May 2023
- Moved to seperate repository