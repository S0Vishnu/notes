
# How to use :

## Gradient Material

``` ts
const material = createGradientMaterial({
	colors: colors,
	distortion: true,
	angle: 0,
	smooth: true,
	distortionStrength: .2,
});
```

## Water Material

``` ts
const waterMaterial = createWaterMaterial({ 
	waterColor: 0x3388ff, 
	foamColor: 0xffffff, 
	waveSpeed: 1.5, 
	waveAmplitude: 0.4, 
	waveFrequency: 0.7, 
	rippleStrength: 6.0, 
});
```

