# Shaders testés et fonctionnels dans shadertoy.com



https://thebookofshaders.com/



Test de guigui

```glsl
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 dmouse = ((iMouse.xy - fragCoord.xy) / iResolution.xy) * 2.0f;
    
    if(length(dmouse) < 0.5f){
     	fragColor = texture(iChannel0, fragCoord.xy / iResolution.xy);   
    }else{
        fragColor = texture(iChannel0, fragCoord.xy / iResolution.xy) * 0.3f;
    }
}
```



Bruit de perlin : 

```glsl
#define PI 3.14159265358979323846

float rand(vec2 co){return fract(sin(dot(co.xy ,vec2(12.9898,78.233))) * 43758.5453);}
float rand (vec2 co, float l) {return rand(vec2(rand(co), l));}
float rand (vec2 co, float l, float t) {return rand(vec2(rand(co, l), t));}

float perlin(vec2 p, float dim, float time) {
	vec2 pos = floor(p * dim);
	vec2 posx = pos + vec2(1.0, 0.0);
	vec2 posy = pos + vec2(0.0, 1.0);
	vec2 posxy = pos + vec2(1.0);
	
	float c = rand(pos, dim, time);
	float cx = rand(posx, dim, time);
	float cy = rand(posy, dim, time);
	float cxy = rand(posxy, dim, time);
	
	vec2 d = fract(p * dim);
	d = -0.5 * cos(d * PI) + 0.5;
	
	float ccx = mix(c, cx, d.x);
	float cycxy = mix(cy, cxy, d.x);
	float center = mix(ccx, cycxy, d.y);
	
	return center * 2.0 - 1.0;
}

// p must be normalized!
float perlin(vec2 p, float dim) {
	
	/*vec2 pos = floor(p * dim);
	vec2 posx = pos + vec2(1.0, 0.0);
	vec2 posy = pos + vec2(0.0, 1.0);
	vec2 posxy = pos + vec2(1.0);
	
	// For exclusively black/white noise
	/*float c = step(rand(pos, dim), 0.5);
	float cx = step(rand(posx, dim), 0.5);
	float cy = step(rand(posy, dim), 0.5);
	float cxy = step(rand(posxy, dim), 0.5);*/
	
	/*float c = rand(pos, dim);
	float cx = rand(posx, dim);
	float cy = rand(posy, dim);
	float cxy = rand(posxy, dim);
	
	vec2 d = fract(p * dim);
	d = -0.5 * cos(d * M_PI) + 0.5;
	
	float ccx = mix(c, cx, d.x);
	float cycxy = mix(cy, cxy, d.x);
	float center = mix(ccx, cycxy, d.y);
	
	return center * 2.0 - 1.0;*/
	return perlin(p, dim, 0.0);
}


float noise(vec2 p, float freq ){
	float unit = iResolution.x / freq;
	vec2 ij = floor(p/unit);
	vec2 xy = mod(p,unit)/unit;
	//xy = 3.*xy*xy-2.*xy*xy*xy;
	xy = .5*(1.-cos(PI*xy));
	float a = rand((ij+vec2(0.,0.)));
	float b = rand((ij+vec2(1.,0.)));
	float c = rand((ij+vec2(0.,1.)));
	float d = rand((ij+vec2(1.,1.)));
	float x1 = mix(a, b, xy.x);
	float x2 = mix(c, d, xy.x);
	return mix(x1, x2, xy.y);
}

float pNoise(vec2 p, int res){
	float persistance = .5;
	float n = 0.;
	float normK = 0.;
	float f = 4.;
	float amp = 1.;
	int iCount = 0;
	for (int i = 0; i<50; i++){
		n+=amp*noise(p, f);
		f*=2.;
		normK+=amp;
		amp*=persistance;
		if (iCount == res) break;
		iCount++;
	}
	float nf = n/normK;
	return nf*nf*nf*nf;
}
```





Test bruit de perlin 1 :

```glsl

void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    float facteur = iMouse.x / iResolution.x;
    
    if(pNoise(fragCoord, 200) < facteur){
        fragColor = vec4(0, 0, 0, 1);
    }else{
        fragColor = vec4(1, 1, 1, 1);
    }
}
```



Test de bruit de perlin 2 :

```glsl
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    float noise = pNoise(fragCoord, 100);
    fragColor = vec4(noise) * texture(iChannel0, fragCoord / iResolution.xy) * vec4(1, 0, 0, 1);
}
```



Test bruit de perlin 3 :

```glsl
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    float speed = 10.0f;
    float facteur = iMouse.x / iResolution.x * speed;
    float noise = pNoise(fragCoord + vec2(1, 1) * iTime * facteur, 4);

    if(noise < 0.1f){
        fragColor = vec4(1, 1, 1, 1);
    }else{
        fragColor = vec4(0, 0, 0, 1);
    }

}
```



Test bruit de perlin 4 :

```glsl
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    float speed = 100.0f;
    float noise = pNoise(fragCoord + vec2(1, 1) * iTime * speed, 4);

    fragColor = noise * texture(iChannel0, fragCoord / iResolution.xy) * vec4(2, 0, 0, 1);

}
```

