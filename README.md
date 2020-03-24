# PyRing

<p align="left">
  <img src="https://github.com/jaycosaur/pyring/workflows/Build%20and%20Test/badge.svg">
</p>

A pure python implementation of a ring buffer with optional factory for alternate memory allocation. Variants included are Blocking (with a read cursor) and Locked (all manipulations are secured with a lock) ring buffers.

<p align="center">
  <img width="200" height="200" src="https://github.com/jaycosaur/pyring/blob/master/img/pyring.png">
</p>

You may not call it a ring buffer, they also go by other names like circular buffer, circular queue or cyclic buffer.

## Installation

`python3 -m pip install pyring`

## Usage

Included are several variations of a ring buffer:

1. `RingBuffer` - The most basic, non-blocking non-locked ring.
2. `LockedRingBuffer` - The same as `RingBuffer` but secured by a lock (handy for multithread / multiproc)
3. `BlockingRingBuffer` - Extension of `RingBuffer` with a read method `next()` which increments a read cursor and the writer cannot advance past the read cursor
4. `BlockingLockedRingBuffer` - The same as `BlockingRingBuffer` but secured by a lock (handy for multithread / multiproc)

### Basic usage with default size and factory

```python
import pyring

# create ring buffer
ring_buffer = pyring.RingBuffer()

# add to ring
ring_buffer.put("Something new!")

# get latest from ring
sequence, value = ring_buffer.get_latest()
print(sequence, value) # 0 Something new!
```

### Custom ring size

```python
import pyring

ring_buffer = pyring.RingBuffer(size=128) # size must be a power of 2
```

### Custom factory

```python
import pyring

# custom factory that takes lists of ints and returns the sum
# must declare the get and set methods
class CustomSumFactory(pyring.RingFactory):
    value: typing.List[int] = []

    def get(self):
        return sum(self.value)

    def set(self, values):
        self.value = values

ring_buffer = pyring.RingBuffer(factory=CustomSumFactory)

ring_buffer.put([1,2,3,4,5])

sequence, value = ring_buffer.get_latest()

print(sequence, value) # 0 15
```

## Examples of Usage

COMING SOON

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License

[MIT](https://choosealicense.com/licenses/mit/)
