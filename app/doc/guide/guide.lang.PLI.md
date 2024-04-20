This guide is intended to be an IBM PL/I Language 101 textbook for open-system programmers.

<style>
  .center { text-align: center; }
  .right  { text-align: right;  }
</style>

# Data type and attributes

- coded arithmetic data (rational numbers)
  - fixed decimal
  - fixed binary signed
  - fixed binary unsigned
  - float decimal
  - float binary
- numeric picture data
- text data
  - characters (single-byte characters)
  - wide characters (UTF-16 characters)
  - graphics (double-byte characters)
- bits

## Coded arithmetic data

Coded arithmetic data items are rational numbers.

- for fixed-point data items, the position of the decimal or binary point is specified by a scaling factor declared for a variable.
- for floating-point data items, the data is in the form of a fractional part and an exponent part.

The *precision* `(p, q)` of a coded arithmetic data item includes the number of digits `p` and the scaling factor `q`.

<table>
<caption>Precision of coded arithmetic data (<i>PRECISION attribute</i>, Chapter 3 [PLI3.5])
<thead>
<tr>
<th><code>declare</code> statement
<th>description
<tbody>
<tr>
<td rowspan="2"><code>fixed dec (p,q)</code>
<td><code>p</code>: the number of significant digits
<tr>
<td><code>q</code>: the assumed position of decimal point
<tr>
<td rowspan="2"><code>fixed bin (p,q)</code>
<td><code>p</code>: the number of significant digits
<tr>
<td><code>q</code>: the assumed position of binary point
<tr>
<td><code>float dec (p)</code>
<td><code>p</code>: the number of significant digits
<tr>
<td><code>float bin (p)</code>
<td><code>p</code>: the number of significant digits
</table>

> `fixed bin (p,q) unsigned` is omitted for simplicity.

The number of digits `p`:
- for fixed-point items, the integer is the number of significant digits
- for floating-point items, the integer is the number of significant digits to be maintained excluding the decimal point (independent of its position)

The scaling factor `q` for fixed-point items (optional, default 0) specifies the assumed position of the decimal or binary point:

- A negative scaling factor (`-q`) specifies an integer, with the point assumed to be located `q` places to the right of the rightmost actual digit.
- A positive scaling factor (`q`) that is larger than the number of digits specifies a fraction, with the point assumed to be located `q` places to the left of the rightmost actual digit.
- In either case, intervening zeros are assumed, but they are not stored; only the specified number of digits is actually stored.

> For floating-point items, the scaling factor is not applicable.

<table>
<caption>Examples of the precision
<thead>
<tr>
<th><code>declare
<th>constant
<th>actually stored
<tbody>
<tr>
<td><code>fixed dec (5,-2)
<td><code>3_141_600
<td rowspan="3" class="center">31416
<tr>
<td><code>fixed dec (5)
<td><code>31_416
<tr>
<td><code>fixed dec (5,4)
<td><code>3.141_6
<tr>
<td><code>float dec (2)
<td><code>15E-23
<td rowspan="4" class="center">according to IEEE
<tr>
<td><code>float dec (3)
<td><code>1.96E+07
<tr>
<td><code>float dec (7)
<td><code>3_141_593E-6
<tr>
<td><code>float dec (9)
<td><code>.003_141_593E3
</table>

The `ALIGNED` data alignment attribute is the default for coded arithmetic data. `ALIGNED` specifies that the data element is aligned on the storage boundary corresponding to its data-type requirement.

<table>
<caption>Alignment of fixed decimal and binary data, Table 28 [PLI3.5]
<thead>
<tr>
<th>fixed type
<th>digits
<th>internally
<th>storage (bytes)
<th>data can begin on
<tbody>
<tr>
<td><code>fixed dec (p,q)
<td>
<td class="center">packed decimal
<td class="center">ceil((p+1)/2)
<td>any byte, 0 &ctdot; 7
<tr>
<td><code>fixed bin (p,q)
<td class="center">1 &le; p &le; 7
<td class="center">one byte
<td class="center">1
<td>any byte, 0 &ctdot; 7
<tr>
<td><code>fixed bin (p,q)
<td class="center">8 &le; p &le; 15
<td class="center">half word
<td class="center">2
<td>byte 0, 2, 4, or 6
<tr>
<td><code>fixed bin (p,q)
<td class="center">16 &le; p &le; 31
<td class="center">full word
<td class="center">4
<td>byte 0 or 4
<tr>
<td><code>fixed bin (p,q)
<td class="center">32 &le; p &le; 63
<td class="center">&mdash;
<td class="center">8
<td>byte 0
</table>

> `fixed bin (p,q) unsigned` has different alignment requirements. For the details, refer to [PLI3.5].

<table>
<caption>Alignment of float decimal and binary data, Table 28 [PLI3.5]
<thead>
<tr>
<th>float type
<th>digits
<th>internally
<th>storage (bytes)
<th>data can begin on
<tbody>
<tr>
<td rowspan="3"><code>float dec (p)
<td class="center">1 &le; p &le; 6
<td class="center">32-bit binary floating-point
<td class="center">4
<td >byte 0 or 4
<tr>
<td class="center">7 &le; p &le; 16
<td class="center">64-bit binary floating-point
<td class="center">8
<td>byte 0
<tr>
<td class="center">17 &le; p
<td class="center">128-bit binary floating-point
<td class="center">16
<td >byte 0
<tr>
<td rowspan="3"><code>float bin (p)
<td class="center">1 &le; p &le; 21
<td class="center">32-bit binary floating-point
<td class="center">4
<td>byte 0 or 4
<tr>
<td class="center">22 &le; p &le; 53
<td class="center">64-bit binary floating-point
<td class="center">8
<td>byte 0
<tr>
<td class="center">54 &le; p
<td class="center">128-bit binary floating-point
<td class="center">16
<td >byte 0
</table>

On the z/OS platform, floating-point computations can be done using either floating-point format:

- [IEEE binary floating-point](https://en.wikipedia.org/wiki/IEEE_754) (new for V3R1)
- [IBM S/370 hexadecimal floating-point](https://en.wikipedia.org/wiki/IBM_hexadecimal_floating-point)

The default is IEEE. All computations are done using IEEE floating-point; variables declared S/370 hexadecimal floating-point will be converted as necessary.

> IEEE decimal floating-point format has been supported since V3R7 (October 2007).

## Text data

## Picture data

## Bits

# References

- [[PLI3.5](http://publibfp.boulder.ibm.com/epubs/pdf/ibm3lr40.pdf)] IBM. *Enterprise PL/I for z/OS Language Reference*, 7th Edition for PL/I 3.5 (SC27-1460-05). 2005. Retrieved April 2024.

