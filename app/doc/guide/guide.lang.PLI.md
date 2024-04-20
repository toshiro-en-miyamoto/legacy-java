# PL/I Language Reference

## Data type and attributes (Chapter 3)

- coded arithmetic data (rational numbers)
  - fixed decimal
  - fixed binary signed
  - fixed binary unsigned
  - float decimal
  - float binary
- numeric picture data
- string data: a sequence of contiguous
  - characters (single-byte characters)
  - wide characters (UTF-16 characters)
  - graphics (double-byte characters)
  - bits

### Coded arithmetics data

Coded arithmetics data items are rational numbers.

- for fixed-point data items, the position of the decimal or binary point is specified by a scaling factor declared for a variable.
- for floating-point data items, the data is in the form of a fractional part and an exponent part.

The *precision* (`p`, `q`) of a coded arithmetic data item includes the number of digits `p` and the scaling factor `q`.

| `declare` statement       | description   |
|---------------------------|---------------|
| `fixed dec (p,q)`         | `p`: the number of significant digits      |
|                           | `q`: the assumed position of decimal point |
| `fixed bin (p,q)`         | `p`: the number of significant digits      |
|                           | `q`: the assumed position of binary point  |
| `fixed bin (p,q) signed`  | non-negative or negative  |
| `fixed bin (p,q) unsign`  | non-negative only         |
| `float dec (p)`           | `p`: the number of significant digits      |
| `float bin (p)`           | `p`: the number of significant digits      |

The number of digits `p`:
- for fixed-point items, the integer is the number of significant digits
- for floating-point items, the integer is the number of significant digits to be maintained excluding the decimal point (independent of its position)

The scaling factor `q` for fixed-point items (optional, default 0) specifies the assumed position of the decimal or binary point:

- A negative scaling factor (`-q`) specifies an integer, with the point assumed to be located `q` places to the right of the rightmost actual digit.
- A positive scaling factor (`q`) that is larger than the number of digits specifies a fraction, with the point assumed to be located `q` places to the left of the rightmost actual digit.
- In either case, intervening zeros are assumed, but they are not stored; only the specified number of digits is actually stored.

For floating-point items, the scaling factor is not applicable.

| `declare` example             | constant example  | value in decimal
|-------------------------------|-------------------|-------
| `fixed decimal (3,-2)`        | `567`             | `___56,700`
| `fixed decimal (3)`           | `567`             | `______567`
| `fixed decimal (3,2)`         | `567`             | `________5.67`
| `fixed decimal (3,3)`         | `567`             | `________0.567`
| `fixed decimal (3,5)`         | `567`             | `________0.00567`
| `fixed binary (7,3)`          | `1011.111B`       | `_,___,_11.875`
| `fixed binary (3)`            | `101B`            | `_,___,__5`
| `float decimal (3)`           | `1.96E+02`        | `______196`
| `float binary (4)`            | `11.01E-2B`       | `________0.8125`

The `ALIGNED` data alignment attribute is the default for coded arithmetics data. `ALIGNED` specifies that the data element is aligned on the storage boundary corresponding to its data-type requirement.

<style>
  .center { text-align: center; }
</style>

<table>
<tr>
<th>fixed type
<th class="center">internally
<th class="center">storage (bytes)
<th>data can begin on
<tr>
<td><code>fixed dec (p,q)
<td class="center">packed decimal
<td class="center">ceil((p+1)/2)
<td>any byte, 0 &ctdot; 7
<tr>
<td><code>fixed bin (p,q) signed</code> 1 &le; p &le; 7
<td rowspan="2" class="center">one byte
<td rowspan="2" class="center">1
<td rowspan="2">any byte, 0 &ctdot; 7
<tr>
<td><code>fixed bin (p,q) unsign</code> 1 &le; p &le; 8
<tr>
<td><code>fixed bin (p,q) signed</code> 8 &le; p &le; 15
<td rowspan="2" class="center">half word
<td rowspan="2" class="center">2
<td rowspan="2">byte 0, 2, 4, or 6
<tr>
<td><code>fixed bin (p,q) unsign</code> 9 &le; p &le; 16
<tr>
<td><code>fixed bin (p,q) signed</code> 16 &le; p &le; 31
<td rowspan="2" class="center">full word
<td rowspan="2" class="center">4
<td rowspan="2">byte 0 or 4
<tr>
<td><code>fixed bin (p,q) unsign</code> 17 &le; p &le; 32
<tr>
<td><code>fixed bin (p,q) signed</code> 32 &le; p &le; 63
<td rowspan="2" class="center">&mdash;
<td rowspan="2" class="center">8
<td rowspan="2">byte 0
<tr>
<td><code>fixed bin (p,q) unsign</code> 33 &le; p &le; 64
</table>

<table>
<tr>
<th>float type
<th class="center">internally
<th class="center">storage (bytes)
<th>data can begin on
<tr>
<td><code>float dec (p)</code> 1 &le; p &le; 6
<td class="center">short floating-point
<td class="center">4
<td >byte 0 or 4
<tr>
<td><code>float dec (p)</code> 7 &le; p &le; 16
<td class="center">long floating-point
<td class="center">8
<td>byte 0
<tr>
<td><code>float dec (p)</code> 17 &le; p
<td class="center">extended floating-point
<td class="center">16
<td >byte 0
<tr>
<td><code>float bin (p)</code> 1 &le; p &le; 21
<td class="center">short floating-point
<td class="center">4
<td>byte 0 or 4
<tr>
<td><code>float bin (p)</code> 22 &le; p &le; 53
<td class="center">long floating-point
<td class="center">8
<td>byte 0
<tr>
<td><code>float bin (p)</code> 54 &le; p
<td class="center">extended floating-point
<td class="center">16
<td >byte 0
</table>

On the z/OS platform, floating-point computations can be done using either floating-point format:

- IBM S/370 hexadecimal floating-point
- IEEE binary floating-point (new for V3R1)

The default is IEEE. All computations are done using IEEE floating-point; variables declared S370 hexadecimal floating-point will be converted as necessary.

> IEEE decimal floating-point format has been supported since V3R7.

### Characters

### Picture data

### Bits

## References

- [[PLI3.5](http://publibfp.boulder.ibm.com/epubs/pdf/ibm3lr40.pdf)] IBM. *Enterprise PL/I for z/OS Language Reference*, 7th Edition for PL/I 3.5 (SC27-1460-05). 2005. Retrieved April 2024.

