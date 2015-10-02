A typical problem when using DXT compression is that it's not possible to obtain pure greys (the same applies to any format that stores RGB colors in 565 bits). This is usually a problem when compressing lightmaps, since white light results in colored patches. However, that can be avoided transforming the colors to YCoCg-R color space as follows:

```
Co = R-B;
t = B + (Co/2);
Cg = G-t;
Y = t + (Cg/2);

s = Y - (Cg / 2);
G = Cg + s;
B = s - (Co / 2);
R = B + Co;
```

An even cheaper alternative is to use the JPEG-LS transform:

```
(R-G, G, B-G)
```

It might not be obvious why that works, but with the following code
it's easy to test that pure greys remain pure greys after the transform:

```
for(int i = 0; i < 256; i++)
{
       int R = i;
       int G = i;
       int B = i;

       int a = (R - G + 255) / 2;
       int b = G;
       int c = (B - G + 255) / 2;

       // Quantize/bitexpand colors.
       a = ((a >> 3) << 3) | (a >> 5);
       b = ((b >> 2) << 2) | (b >> 6);
       c = ((c >> 3) << 3) | (c >> 5);

       R = (2 * (a - 123)) + b;
       G = b;
       B = (2 * (c - 123)) + b;

       printf("%d: %d %d %d\n", i, R, G, B);
}
```

Note that signed components are packed as unsigned integers, so they need to be scaled and biased. In order to obtain pure greys, the bias that you have to use to reconstruct the color is equal to 255/2 quantized to 5 bits, that's why 123 is used. While this method allows you to obtain pure greys, it introduces more errors in the colors away from grey. So, it may not always be a good idea to use it.