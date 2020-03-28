# Metodología de las proyecciones

En la sección Proyecciones hemos presentado estimaciones del número de infectados bajo distintos escenarios de contención de la población. Estas predicciones fueron hechas utilizando un modelo matemático.

El modelo que utilizamos en este sitio es una simplificación del propuesto por [Alex Arenas, Jesús Gómez-Gardeñes](https://covid-19-risk.github.io/map/) y sus colaboradores. Para ver el detalle técnico puedes ir [aquí](https://covid-19-risk.github.io/map/model.pdf). Escogimos seguir esta propuesta debido a la precisión con la que describe el caso español.
A continuación presentamos una breve explicación de la versión que hemos implementado.

>Descargo de responsabilidad. El carácter de nuestras predicciones es meramente ilustrativo, nuestro equipo *NO* está formado por epidemiólogos.

## ¿Cómo predecir el número de infectados?

La dinámica de una epidemia es un fenómeno azaroso. Las personas se contagian entre sí sin darse cuenta. Basta con que alguna partícula del virus encuentre su camino hacia los ojos o la boca para que una persona nueva sea infectada. Una vez que una persona se infecta empezará la progresión de la enfermedad dentro de ella. Para poder modelar entonces cómo se propaga el virus dentro de una población es necesario conocer no sólo cómo afecta a un individuo, sino también es importante conocer cómo el comportamiento humano beneficia o perjudica la propagación del virus. Cualquier modelo epidemiológico tomará en cuenta estos dos factores. Por un lado, hay que tener claras las diferentes etapas que atraviesa un individuo afectado, es decir si genera síntomas o no, si requiere de atención médica o no y qué tan pronto se recuperará. Por otro lado, hay que estimar a cuántas personas un individuo infectado puede contagiar. Esto dependerá fuertemente del tamaño de la población, de cómo esté distribuida y de los hábitos de las personas en el día a día. Entender la relación entre estos dos elementos es lo que nos permite generar una predicción de cómo aumentará el número de casos.

Siempre que se modela una epidemia (o cualquier fenómeno en general) es necesario hacer simplificaciones de la realidad. De alguna forma esto equivale a escribir una buena historia muy estructurada no necesariamente idéntica a la realidad pero que se le parezca lo suficiente para que nos permita hacer predicciones razonables. Por ende, nuestro modelo matemático equivale a contar una versión simplificada de cómo se contagia el coronavirus de persona a persona y de cómo afecta a la persona que lo contrae.
Lo más difícil es saber qué tan probable es que un portador del virus contagie a alguien más. Es muy complicado sobretodo porque los encuentros en el día a día entre personas son muy diferentes de persona a persona. Por eso, en vez de concentrarnos en un modelo que describa individuos, nos centramos en un modelo que describe a toda una población al mismo tiempo.  Ésta es nuestra primera simplificación. Es muy útil ya que el comportamiento de un grupo es más predecible que el de un individuo. La segunda aproximación es que asumimos que los portadores del virus están bien mezclados en la población. Es decir, que la probabilidad de que alguien que me encuentre sea infectado es siempre la misma, dónde y cuándo sea que lo vea. Esta aproximación puede sonar exagerada, pero sin ella es difícil hacer la predicción. Con estas suposiciones es fácil estimar el número de nuevos contagios en un día.

Una vez que hemos idealizado cómo se contagian las personas debemos simplificar qué es lo que ocurre después. Dada la experiencia con el coronavirus en los últimos meses en otros países como China y Corea del Sur, asumimos que es muy probable que pasen 2 o 3 días antes de que las personas contagiadas empiecen a ser contagiosas. A partir de ese momento podrán infectar a otras personas, pero pasarán 2 o 3 días antes de que muestren síntomas. También sabemos que una vez que los síntomas aparecen algunas personas no se sentirán muy mal, y puede que se queden en casa o que sigan haciendo su vida normal. Si siguen haciendo su vida normal seguirán infectando nuevas personas.

Del grupo de personas infectadas, un gran número se recuperará después de varios días sin necesidad de recibir atención en un hospital. Sin embargo, de las personas que presentan síntomas, una pequeña fracción suele ponerse más grave llegando a necesitar de cuidado intensivos en un hospital. De las personas en cuidados intensivos una parte se recupera después de varios días y la otra tristemente fallece.

Ante la amenaza a la salud pública que respresenta el coronavirus, muchos países han tomado acciones para frenar su avance. Esto se incluye en el modelo considerando que a partir de cierto día una fracción de la población sana se aisla en su casa sin salir.  

Ésta es nuestra versión simplificada de la epidemia de coronavirus. Así pues, en el modelo todas las personas forman parte de alguno de los siguientes grupos.

- **S**usceptibles   - Las personas que *no* han contraído el virus, pero es posible que se contagien al entrar en contacto con algún portador.

- **E**xpuestos - Las personas que *sí* han contraído el virus pero *no* son contagiosas.  Consideramos que les toma 2 o 3 días pasar al siguiente grupo.


- **A**sintomáticos - Las personas que *sí* han contraído el virus, *sí* son contagiosas, pero *no* presentan síntomas.  

- **I**nfectados - Las personas que *sí* presentan síntomas y *sí* son contagiosas. Lo más probable es que se recuperen pronto en casa.

- **H**ospitalizados - presentan síntomas graves y requieren de cuidados intensivos.

- **C**ontenidos - Las personas sanas que se aislaron en casa a partir de la instrucción del gobierno.

- **R**ecuperados - Las personas que se recuperan de coronavirus, ya sea en casa o en el hospital.

- **F**allecidos  - Las personas que han fallecido *por* coronavirus después de estar en cuidados intensivos.

>Las anteriores definiciones son sólo para el modelo de coronavirus, NO son características generales de una enfermedad.

Representando a cada grupo por la primera letra de su nombre, el modelo se puede representar con el diagrama a continuación.  Las flechas sólidas indican las etapas que puede atravesar un individuo y los textos indican los tiempos y probabilidades para pasar de una a la otra.

![Diagrma del modelo](https://github.com/aguirreFabian/covid/blob/master/images/diagrama.001.jpeg "Diagrama del modelo")  

Para poder estimar el número de nuevos contagios cada día es necesario tener cierta información cuantitativa. Es decir, no basta sólo con conocer la estructura mostrada en el diagrama, sino también hay que conocer los números sobre las flechas, los llamados parámetros del modelo. Estos son: los tiempos de transición de un grupo a otro, la probabilidad de contagio, la fracción de la población aislada, la probabilidad de ser hospitalizado y la probabilidad de recuperarse. Conociendo esto es posible hacer una predicción razonable por al menos una semana. Cabe mencionar que todos los parámetros utilizados por nosotros se han obtenido de observaciones en otros países reportados en diferentes trabajos (ver Referencias). Utilizamos prácticamente los mismos valores de los parámetros que en el trabajo de [Arenas](https://covid-19-risk.github.io/map/model.pdf) y colaboradores. Sólo modificamos aquéllos relacionados con la estructura poblacional de México. Para ello recurrimos a los datos reportados por el [INEGI](https://www.inegi.org.mx/).

Una vez conocidos los parámetros, para hacer una predicción a futuro es necesario saber la cantidad de gente en cada grupo al inicio de la epidemia. La cantidad de suscetibles inicial es prácticamente la población del estado. La cantidad de infectados, recuperados, hospitalizados y fallecidos es posible de conocer a través de los datos hecho públicos por la [Secretaría de Salud](https://www.gob.mx/salud/documentos/coronavirus-covid-19-comunicado-tecnico-diario-238449) y [notas periodísticas](https://www.eluniversal.com.mx/mundo/coronavirus-covid-19). Es muy importante señalar que la cantidad de gente portadora, ya sea expuesta o asintomática, es imposible de conocer. Ésta es la única cantidad que tenemos que adivinar, por lo que escogemos el valor que ajusta mejor con los datos de los días siguientes al inicio de la epidemia. Esta acción en si misma se puede entender como una estimación burda del número de portadores no detectados.

De igual modo, es imposible saber cuál es la fracción de la población que se ha aislado por completo. Por eso simplemente presentamos diferentes posibilidades de evolución de la epidemia dependiendo de cuanta gente se autoaisle.


Para hacer el modelo un poco más realista consideramos a las poblaciones de cada estado por su cuenta. Es decir, simulamos la evolución de cada estado de manera independiente y al final juntamos los resultados. Los números mostrados en la sección de Proyecciones son la suma de las 32 simulaciones independientes.

Para hacer el modelo un poco más realista se podría tomar en cuenta la movilidad de personas entre estados, pero por ahora no lo estamos incluyendo. Ésta es una modificación que se puede hacer en el futuro de contar con datos de movilidad diaria entre estados. En el grupo de [Arenas](https://covid-19-risk.github.io/map/) sí lo pudieron tener en cuenta hasta el día en que se declaró el estado de alarma ya que a partir de ese momento la movilidad en España cambió pero de eso ya no se tienen datos (actualizado al 27 de marzo de 2020).

## Referencias  

1. Arenas, Alex, et al. "A mathematical model for the spatiotemporal epidemic spreading of COVID19." medRxiv (2020).  (https://www.medrxiv.org/content/10.1101/2020.03.21.20040022v1)

1. Li, Qun, et al. "Early transmission dynamics in Wuhan, China, of novel coronavirus–infected pneumonia." New England Journal of Medicine (2020).  (https://www.nejm.org/doi/full/10.1056/NEJMoa2001316)

1. Read, Jonathan M., et al. "Novel coronavirus 2019-nCoV: early estimation of epidemiological parameters and epidemic predictions." MedRxiv (2020). (https://www.medrxiv.org/CONTENT/10.1101/2020.01.23.20018549V2)

1. Danon, Leon, et al. "A spatial model of CoVID-19 transmission in England and Wales: early spread and peak timing." medRxiv (2020). (https://www.medrxiv.org/content/10.1101/2020.02.12.20022566v1)  

1. Bi, Qifang, et al. "Epidemiology and Transmission of COVID-19 in Shenzhen China: Analysis of 391 cases and 1,286 of their close contacts." medRxiv (2020). (https://www.medrxiv.org/content/10.1101/2020.03.03.20028423v3)  

1. Wilson, N., et al. "Case-Fatality Risk Estimates for COVID-19 Calculated by Using a Lag Time for Fatality." Emerging infectious diseases 26.6 (2020). (https://www.ncbi.nlm.nih.gov/pubmed/32168463)  

1. Ferguson, Neil M., et al. "Impact of non-pharmaceutical interventions (NPIs) to reduce COVID-19 mortality and healthcare demand." Imperial College, London (2020). (https://doi.org/10.25561/77482)  

1. Prem, Kiesha, Alex R. Cook, and Mark Jit. "Projecting social contact matrices in 152 countries using contact surveys and demographic data." PLoS computational biology 13.9 (2017): e1005697. (https://journals.plos.org/ploscompbiol/article?rev=2&id=10.1371/journal.pcbi.1005697)  

1. [INEGI](https://www.inegi.org.mx/) Para datos poblacionales de México.

1. [Informe Secretaría de Salud](https://www.gob.mx/salud/documentos/coronavirus-covid-19-comunicado-tecnico-diario-238449)

1. Reporte especial de coronavirus del periódico [El Universal](https://www.eluniversal.com.mx/mundo/coronavirus-covid-19)
