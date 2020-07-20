---
title: "Earthquakes around the world"
date: 2020-07-19T22:18:30-04:00
featured_image: /images/earthquake.jpg
summary: "How a random tweet ignited a passion for the *analytica* in my heart. The findings of analyzing 86K+ earthquakes."
draft: false

---

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.9.3/Chart.min.css" integrity="sha512-/zs32ZEJh+/EO2N1b0PEdoA10JkdC3zJ8L5FTiQu82LR9S/rOQNfQN7U59U9BC12swNeRAz3HSzIL2vpp4fv3w==" crossorigin="anonymous" />

Hace un par de años alguien twitteó que Chile era afortunado porque todos los terremotos habían sido de noche, en fin de semana o algo así, entonces no interrumpía mucho la vida cotidiana y era más fácil evacuar y estar a salvo. Aparte de olvidarse del terremoto del 2010 y sus víctimas por el tsunami me pareció que tampoco tenía idea del *sesgo de la disponibilidad*.

El sesgo o [heurística de disponibilidad](https://es.wikipedia.org/wiki/Heur%C3%ADstica_de_disponibilidad) es aquel que todos sufrimos cuando nos preguntan por ejemplos de algo. Nuestro cerebro **NO** nos entrega una muestra estadísticamente representativa de la realidad, sino que los ejemplos que más nos han impactado, gustado, o por casualidad nos hemos fijado. Generalmente estos sesgos además se van alimentando así mismos, porque cuando nos llega nueva información y la comparamos con lo que tenemos en la mente, nos decimos *“¡todo calza!”*. No los sigo aburriendo, si quieren saber más lea [wikipedia](https://es.wikipedia.org/wiki/Heur%C3%ADstica_de_disponibilidad) o, mejor aún, el libro del Nobel de economía [Daniel Kahneman](https://en.wikipedia.org/wiki/Daniel_Kahneman), *[“Thinking, fast and slow”](https://www.amazon.com/Thinking-Fast-Slow-Daniel-Kahneman/dp/0374533555)*.

Sabiendo que el susodicho estaba sufriendo de ese sesgo, pero sin contar con evidencia histórica para decirle algo con fundamentos, se me ocurrió la “genial” idea de analizar la hora, día de la semana, mes, estación, etc de los terremotos relevantes de los últimos 120 años.

{{< giphy bWM2eWYfN3r20>}}
*¡Llévame a lo bueno! Si no quieren leer toda la historia, vayan al final a ver los gráficos.*

Lo primero era encontrar una buena base de datos y para eso encontré la de la *U.S. Geological Survey*, o [USGS](https://www.usgs.gov/) pa' los amigos, que tiene unas [APIs magníficas](https://www.usgs.gov/products/data-and-tools/apis) para bajar lo que quieras. Habiendo bajado todos los terremotos de **más de 5 grados** con sus coordenadas y hora en gmt+0 me puse a analizar superficialmente. A nadie le sorprenderá que la hora de los terremotos distribuye básicamente uniforme durante el día (link a distribución uniforme) y los días de la semana.

{{< figure src="/images/uniform.png" title="Ejemplo teórico de cómo se ve la distribución de los terremotos sobre la hora del día." >}}

Como ya me había tomado una semana para hacer algo tan básico no le iba a responder el tweet, pero sí desafiarme a mi mismo.

*¿Qué pasa con las horas locales, solo analizaste en base a gmt? ¿Qué pasa con la estación del año? Analizaste hasta temblorcitos de 5 grados, ¿qué pasa con los terremotos relevantes que asustan a la gente?*

[Como saben](/2020/toilet-paper/), cuando me pongo a pensar en algo recibo la misma satisfacción de mi cerebro cómo si de verdad la hiciera, así que todas esas preguntas quedaron sin respuesta (me imaginé respondiéndolas, entonces la respuesta fue irrelevante). Pero, como también saben o **debieran saber** estamos viviendo una pandemia global que nos tiene a todos más o menos aislados y a algunos con un poco más de tiempo para hacer [tonteras](http://toiletpaperorientation.org/).

Así, les presento el análisis y el camino recorrido para llevarlo a cabo.

Camino recorrido
==
Ajustando los datos
--

Lo primero a hacer era ajustar la hora del terremoto o temblor a su hora local. La base de *USGS* viene con ubicación, pero no para todos y en un formato no uniforme. **Sí viene con las coordenadas**, así que era cosa de encontrar la zona horaria para ellas. La cosa era cómo.

Usando un *[sitio web especializado](http://google.com/)*, encontré varios hilos en *Stack Overflow* que hablaban de la misma necesidad y se nombraban varias APIs públicas. Después de un profundo análisis (era la más fácil y más gratis) elegí **[GeoNames](https://www.geonames.org/export/)** que tiene una API para exactamente lo que quería. Solo había un problema, cómo usar esa API para ajustar los 86.184 terremotos en la base...

Esta vez no cometí [errores del pasado](/2020/toilet-paper/), así que programé 4 líneas en php para que hiciera el ajuste por mí. El único tema era mi conexión a internet (un prepago Entel 3g, porque a la antena no le da pa 4g) y la velocidad de la API de GeoNames (además de solo permitir 20.000 consultas diarias). Dejaba el código corriendo mientras trabajaba, pero al dejar el computador inevitablemente tenía que detenerlo o la inestabilidad de la conexión lo mataba. En algún momento saqué la cuenta del tiempo que se iba a demorar y eran **11 días corriendo las 24 horas**. Por suerte mejoró y solo fueron unos 5 días, o alrededor de *43 horas* si lo hubiera dejado corriendo.

Analizando
--

Ya tenía los datos ajustados a su zona horaria, ya tenía las coordenadas pa saber si era invierno o verano, ¡podía empezar mi análisis!

Lo primero era ver con respecto a la hora del día, ¿hay alguna tendencia a que sean de noche? ¡NO! Distribuyen bastante uniforme...
<div style="height: 300px">
    <canvas id="allTimes" width="400" height="400"></canvas>
</div>

*"Ya, pero la persona que twiteó hablaba de los terremotos. ¿Qué pasa si sólo miras los terremotos de más de 7 grados?”*. Lo que pasa, ¡es que distribuyen igual de uniforme! Ese peak a las 11 no cambia mucho la cosa.
<div style="height: 300px">
    <canvas id="7Times" width="400" height="400"></canvas>
</div>

*“OK, pero estás mirando 120 años de historia, ¿cómo salen los últimos 40 años, que son la base del argumento del susodicho?”* No es tan claro, porque son muy pocos datos, pero a simple vista no hay cosas raras… Bastante uniforme.
<div style="height: 300px">
    <canvas id="7Times40" width="400" height="400"></canvas>
</div>

En resumen, les puedo contar que no hay ninguna cosa rara con la hora o fecha de los terremotos. Hay una probabilidad estadística totalmente uniforme para la estación del año, la hora del día o el día de la semana para que ocurra un terremoto, relevante (>7 grados) o no.

Hay algunas excepciones, como el hecho de que **nunca ha habido un terremoto de más de 8 grados a las 0 ó 2 hora local**, pero tampoco ha habido muchos terremotos de más de grado 8 en la historia (*son 71 en total*). Quizás sea merecedor de ser un [Dato Freak](https://www.datosfreak.org/).
<div style="height: 300px">
    <canvas id="8Times" width="400" height="400"></canvas>
</div>

Ahora, les dejo algunos gráficos pa que esto no sea tan fome (aburrido para los no *chileno-parlantes*):
</br>*<sub><sup>Nota: Para las estaciones del año y pensando que la temperatura puede ser un factor relevante, sólo se consideran los terremotos más al sur o norte de los trópicos. Los meses "calientes" para el hemisferio norte son entre Abril y Septiembre, para el sur son entre Octubre y Marzo.</sub></sup>*
</br>*<sub><sup>Nota2: Los terremotos de más de 8 tienen saltos "entretenidos" porque son muy pocos y poco representativos, no dejes que tu cerebro te haga creer tonteras. </sub></sup>*
<div style="height: 300px">
    <canvas id="month" width="400" height="400"></canvas>
</div>
<div style="height: 300px">
    <canvas id="month8" width="400" height="400"></canvas>
</div>
<div style="height: 300px">
    <canvas id="monthnh" width="400" height="400"></canvas>
</div>
<div style="height: 300px">
    <canvas id="monthsh" width="400" height="400"></canvas>
</div>
<div style="height: 300px">
    <canvas id="weeks" width="400" height="400"></canvas>
</div>
<div style="height: 300px">
    <canvas id="weekday" width="400" height="400"></canvas>
</div>
<div style="height: 300px">
    <canvas id="seasons" width="400" height="400"></canvas>
</div>

Conclusiones y futuros desarrollos
--
En conclusión, **no hay evidencia suficiente que indique una correlación entre la hora del día, el día de la semana, el mes del año o la estación y la ocurrencia de un terremoto**.

*¿Queda alguna pregunta por hacer? ¿Algún gráfico que sea fácil de hacer y pueda subir acá? ¿Algún análisis que falte?* Por ejemplo, *¡cómo variaron las temperaturas del día anterior, el día del terremoto y el día siguiente!* Mi intuición es que **no habrá nada relevante**. Puedes recrear los pasos de este post y responder esas preguntas tú mismo.

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.9.3/Chart.bundle.min.js" integrity="sha512-vBmx0N/uQOXznm/Nbkp7h0P1RfLSj0HQrFSzV8m7rOGyj30fYAOKHYvCNez+yM8IrfnW0TCodDEjRqf6fodf/Q==" crossorigin="anonymous" type="text/javascript"></script>

<script type="text/javascript">
var ctx = document.getElementById('allTimes').getContext('2d');
var myChart = new Chart(ctx, {
    type: 'bar',
    data: {
        labels: ['0','1','2','3','4','5','6','7','8','9','10','11','12','13','14','15','16','17','18','19','20','21','22','23'],
        datasets: [{
            label: 'Earthquakes per local hour',
            data: [3574,3546,3596,3633,3741,3650,3503,3632,3556,3624,3610,3629,3572,3676,3511,3519,3604,3487,3594,3556,3578,3530,3583,3680],
            backgroundColor: 'rgba(255, 99, 132, 0.2)',
            borderColor: 'rgba(255, 99, 132, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var ctx2 = document.getElementById('7Times').getContext('2d');
var myChart2 = new Chart(ctx2, {
    type: 'bar',
    data: {
        labels: ['0','1','2','3','4','5','6','7','8','9','10','11','12','13','14','15','16','17','18','19','20','21','22','23'],
        datasets: [{
            label: 'Earthquakes per local hour, mag 7 and above',
            data: [48,38,45,48,53,47,37,43,44,40,41,69,46,47,47,41,46,55,45,42,43,37,42,54],
            backgroundColor: 'rgba(54, 162, 235, 0.2)',
            borderColor: 'rgba(54, 162, 235, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});

var ctx3 = document.getElementById('7Times40').getContext('2d');
var myChart3 = new Chart(ctx3, {
    type: 'bar',
    data: {
        labels: ['0','1','2','3','4','5','6','7','8','9','10','11','12','13','14','15','16','17','18','19','20','21','22','23'],
        datasets: [{
            label: 'Earthquakes per local hour, mag 7 and above, last 40 years',
            data: [14,19,14,19,20,17,14,23,16,8,13,31,19,23,20,20,16,23,24,19,20,16,14,29],
            backgroundColor: 'rgba(255, 206, 86, 0.2)',
            borderColor: 'rgba(255, 206, 86, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var ctx4 = document.getElementById('8Times').getContext('2d');
var myChart4 = new Chart(ctx4, {
    type: 'bar',
    data: {
        labels: ['0','1','2','3','4','5','6','7','8','9','10','11','12','13','14','15','16','17','18','19','20','21','22','23'],
        datasets: [{
            label: 'Earthquakes per local hour, mag 8 and above',
            data: [0,4,0,4,7,2,2,4,1,3,4,6,1,2,6,2,4,1,4,3,4,3,1,3],
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var cmonth = document.getElementById('month').getContext('2d');
var month = new Chart(cmonth, {
    type: 'bar',
    data: {
        labels: ['January','February','March','April','May','June','July','August','September','October','November','December'],
        datasets: [{
            label: 'Earthquakes per month',
            data: [7409,6818,7810,7007,6901,6755,7069,7169,7032,7251,7346,7617],
            backgroundColor: 'rgba(153, 102, 255, 0.2)',
            borderColor: 'rgba(153, 102, 255, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var cmonth8 = document.getElementById('month8').getContext('2d');
var month8 = new Chart(cmonth8, {
    type: 'bar',
    data: {
        labels: ['January','February','March','April','May','June','July','August','September','October','November','December'],
        datasets: [{
            label: 'Earthquakes magnitude 8 and above per month',
            data: [3,5,7,8,8,4,2,8,8,3,7,8],
            backgroundColor: 'rgba(255, 159, 64, 0.2)',
            borderColor: 'rgba(255, 159, 64, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var cmonthnh = document.getElementById('monthnh').getContext('2d');
var monthnh = new Chart(cmonthnh, {
    type: 'bar',
    data: {
        labels: ['January','February','March','April','May','June','July','August','September','October','November','December'],
        datasets: [{
            label: 'Earthquakes magnitude 8 and above per month, Northern Hemisphere',
            data: [2,2,6,4,2,1,1,4,4,2,5,5],
            backgroundColor: 'rgba(255, 99, 132, 0.2)',
            borderColor: 'rgba(255, 99, 132, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var cmonthsh = document.getElementById('monthsh').getContext('2d');
var monthsh = new Chart(cmonthsh, {
    type: 'bar',
    data: {
        labels: ['January','February','March','April','May','June','July','August','September','October','November','December'],
        datasets: [{
            label: 'Earthquakes magnitude 8 and above per month, Southern Hemisphere',
            data: [1,3,1,4,6,3,1,4,4,1,2,3],
            backgroundColor: 'rgba(54, 162, 235, 0.2)',
            borderColor: 'rgba(54, 162, 235, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var cweeks = document.getElementById('weeks').getContext('2d');
var weeks = new Chart(cweeks, {
    type: 'bar',
    data: {
        labels: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52],
        datasets: [{
            label: 'Earthquakes per week',
            data: [1700,1613,1676,1703,1686,1649,1584,1773,1690,1990,1702,1720,1823,1526,1737,1606,1436,1608,1588,1598,1495,1485,1632,1662,1534,1554,1454,1581,1690,1644,1606,1658,1758,1552,1616,1666,1640,1594,1672,1778,1489,1661,1577,1564,1745,1806,1787,1581,1720,1567,1797,1396],
            backgroundColor: 'rgba(255, 206, 86, 0.2)',
            borderColor: 'rgba(255, 206, 86, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var cweekday = document.getElementById('weekday').getContext('2d');
var weekday = new Chart(cweekday, {
    type: 'bar',
    data: {
        labels: ['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday'],
        datasets: [{
            label: 'Earthquakes per weekday',
            data: [12145,12449,12272,12255,12563,12339,12161],
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
var cseasons = document.getElementById('seasons').getContext('2d');
var season = new Chart(cseasons, {
    type: 'bar',
    data: {
        labels: ['Frío Norte','Calor Norte','Frío Sur','Calor Sur'],
        datasets: [{
            label: 'Earthquakes per "Season" and Hemisphere',
            data: [12346,11308,7123,7400],
            backgroundColor: 'rgba(153, 102, 255, 0.2)',
            borderColor: 'rgba(153, 102, 255, 1)',
            borderWidth: 1
        }]
    },
    options: {
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
</script>
