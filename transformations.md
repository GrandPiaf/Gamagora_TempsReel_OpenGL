# Transformations



## Translation

```
T = (tx, ty, tz)
p' = p + T
p' = (
	x + tx,
	y + ty,
	z + tz
)
```



## Scale

```
S = (sx, sy, sz)
p' = p * S
p' = (
	x * sx,
	y * sy,
	z * sz
)
```



## Rotation

```
Rotation around axis Z of angle A
R = (Z, A)
p'_carthesion = (x, y, z)
p'_polaer = (r, a, z)

p'_polar = (
	r,
	a + A
	z
)

R = ( cos A, -sin A, 0
	  sin A, cos A, 0
	  0    , 0    , 1	)
	  
p' = R .*. p
```



## Projection

```
P = f

p' = (
	x * f / Z
	y * f / Z
	z
)
```







## PROBLEM

- 1st problem : composition is <u>costly</u>

```
p_camera = Rotation_camera * (translation_camera + Rotation_knight * (translation_knight + rotation_knight_arm *(translation_knight_arm + rotation_sword * (translation_sword * p_sword ))))
```



- represent everything using the same mathematical operation

```
p' = (T ?*? T' ?*? T'') ?*? p

MASTERT = T ?*? T' ?*? T''

p' = MASTERT ?*? p

-----------------------------------------------------------------------------------------------------------------------------------------

Homogeneous coordinates
-----------------------

p = (x, y, z, w)_4 = (x / w, y / w, z / w)_3
Example : p = (10, 5, 1)_3 = (10, 5, 1, 1)_4 = (20, 10, 2, 2)_4

-----------------------------------------------------------------------------------------------------------------------------------------


- Translation
T = (
	1, 0, 0, dx
	0, 1, 0, dy
	0, 0, 1, dz
	0, 0, 0, 1
)

p' = T * p

p' = (
	1 * x  +  0 * y  +  0 * z  +  w * dx
	0 * x  +  1 * y  +  0 * z  +  w * dy
	0 * x  +  0 * y  +  1 * z  +  w * dz
	0 * x  +  0 * y  +  0 * z  +  w * 1
) = (x + dx, y + dy, z + dz, 1)



- Rotation

R = (
	cos A, -sin A, 0, 0
	sin A, cos A , 0, 0
	0    , 0     , 1, 0
	0    , 0     , 0, 1
)



-Scale

S = (
	sx, 0, 0, 0
	0, sy, 0, 0
	0, 0, sz, 0
	0, 0, 0, 1)
	


- Projection

P = (
	f, 0, 0, 0
	0, f, 0, 0
	0, 0, 1, 0
	0, 0, 0, 1
)

p' = (fx, fy, 1, z) = (fx / z, fy / z, 1, 1)


----


P_camera = Rotation * translation * Rotation' * Translation' * Rotation'' * Translation'' .... * P
```





