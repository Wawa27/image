## Transformations ponctuelles

#### Transformation linéaire simple à une image

```javascript
    run("Duplicate...", "title=gateaux1-tr.png");
    getStatistics(area, mean, min, max, std);
    for (i = 0; i < getHeight(); i++) {
        for (j = 0; j < getWidth(); j++) {
            iprime = 255 / (max - min) * (getPixel(j, i) - min);
            setPixel(j, i, iprime);
        }
    }
```

![img.png](img.png)

Histogramme de l'image originale
![img_4.png](img_4.png)

Histogramme de l'image transformée
![img_5.png](img_5.png)

On peut voir

#### Transformation linéaire avec saturation à une image

```javascript
    run("Duplicate...", "title=gateaux1-tr-sat.png");
    getStatistics(area, mean, min, max, std);
    smin = 1
    smax = 180
    for (i = 0; i < getHeight(); i++) {
        for (j = 0; j < getWidth(); j++) {
            iprime = 255 / (smax - smin) * (getPixel(j, i) - smin);
            setPixel(j, i, iprime);
        }
    }
```

![img_1.png](img_1.png)

Histogramme de l'image originale
![img_4.png](img_4.png)

Histogramme de l'image transformée
![img_3.png](img_3.png)

On peut voir une plus grande répartition des niveaux de gris (0 à 255 contre 0 à 201 pour l'originelle), avec une
moyenne 2 presque fois plus grande (76 contre 131)

#### Transformation affine

```javascript
    run("Duplicate...", "title=gateaux1-tr-affine.png");
    getStatistics(area, mean, min, max, std);
    a=2
    b=127
    for (i = 0; i < getHeight(); i++) {
        for (j = 0; j < getWidth(); j++) {
            iprime = a * getPixel(j, i) + b;
            setPixel(j, i, iprime);
        }
    }
```

a=0.1 b=30
![img_9.png](img_9.png)
![img_6.png](img_6.png)

a=5 b=0
![img_8.png](img_8.png)
![img_7.png](img_7.png)

a=2 b=127
![img_10.png](img_10.png)
![img_11.png](img_11.png)

On peut conclure que la valeur de a définit l'éspacement dans l'histogramme, tandis que la valeur de b définit le début
des valeurs dans l'histogramme.

#### Contraste maximum

```javascript
    run("Duplicate...", "title=gateaux1-tr-contraste-max.png");
    getStatistics(area, mean, min, max, std);
    for (i = 0; i < getHeight(); i++) {
        for (j = 0; j < getWidth(); j++) {
            if (getPixel(j, i) < 30) {
                setPixel(j, i, 0);
            } else {
                iprime = 255 / (max-min) * (getPixel(j, i) - min);
                setPixel(j, i, iprime);
            }
        }
    }
```
![img_13.png](img_13.png)
![img_14.png](img_14.png)

## Égalisation d'histogramme

```javascript
    run("Duplicate...", "title=gateaux1-egalise.png");
    getHistogram(values, counts, 256);
    xValues = newArray(values.length);
    yValues = newArray(values.length);
    sum = 0;
    for (i = 0; i < 256; i++) {
        xValues[i] = i;
        yValues[i] = sum + counts[i];
        sum += counts[i];
    }
    
    for (i = 0; i < getHeight(); i++) {
        for (j = 0; j < getWidth(); j++) {
            hci = yValues[getPixel(j, i)];
            iprime = 256 / (getWidth() * getHeight()) * hci - 1;
            setPixel(j, i, iprime);
        }
    }
    
    getHistogram(values, counts, 256);
    xValues = newArray(values.length);
    yValues = newArray(values.length);
    ySumValues = newArray(values.length);
    sum = 0
    for (i = 0; i < 256; i++) {
        xValues[i] = i;
        yValues[i] = counts[i];
        ySumValues[i] = sum + counts[i];
        sum += counts[i];
    }
    
    Plot.create("Histogramme", "i", "i'", xValues, yValues);
    Plot.show();
    Plot.create("Histogramme Cumulées", "i", "i'", xValues, ySumValues);
    Plot.show();
```

Histogramme avant égalisation
![img_16.png](img_16.png)

Histogramme cumulées avant égalisation
![img_17.png](img_17.png)

Histogramme apres l'égalisation

![img_19.png](img_19.png)

Histogramme cumulées apres l'égalisation

![img_18.png](img_18.png)