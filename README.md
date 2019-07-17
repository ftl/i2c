# I2C under Linux in Go

This is a library to use the I2C bus e.g. on the Raspberry Pi in Go.

## Disclaimer

I develop this library for myself and just for fun in my free time. If you find it useful, I'm happy to hear about that. If you have trouble using it, you have all the source code to fix the problem yourself (although pull requests are welcome). 

## Usage

```
// open communication with the device at address 0x60 on bus number 1
bus := i2c.Open(0x60, 1) 

// write one byte to register 0x1
bus.WriteReg(0x1, 0x23)

// read one byte from register 0x2
buf := make([]byte, 1)
bus.ReadReg(0x2, buf)

// get a writer that writes to register 0x3 and the following registers
w := bus.RegWriter(0x3)
someFancyFuncThatWritesToAWriter(w)

// read n bytes from register 0xa and the following registers
buf := make([]byte, n)
bus.ReadReg(0xa, buf)

// check if any error happened in the meantime
if bus.Err() != nil {
    log.Fatal(bus.Err())
}
```

### Error Handling

The bus keeps the first error that happened when using any of the methods that can return an error. After this first error occurred, all following method invocations will do nothing and just return this error.

This allows to make several calls and have only one error check after the last call. Less error checking boilerplate.

## License

This software is published under the [MIT License](https://www.tldrlegal.com/l/mit).

Copyright [Florian Thienel](http://thecodingflow.com/)