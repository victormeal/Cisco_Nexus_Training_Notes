when trying to redistribute routes using an access list we have to use an specific format:

Example:

For /24==255.255.255.0

We want to allow from the final octect from 0 to 255

The final octect we have to use a wildmask

0 = 00000000
.
.
.
.
192 = 11000000

224 = 11100000

240 = 11110000

248 = 11111000

252 = 11111100

255 = 1111111

If you see all the bits change from 0 to 255 so we have to use wild mask 0.0.0.255

As an example to advertise
10.177.50.0/24

We have to use the following:

access-list TAC permit ip 10.177.50.0 0.0.0.255 255.255.255.0 0.0.0.255
