# Linjär regression: huspriser

## 1. Datainhämtning 

Med Sverige som exempel så finns det flera privata och offentliga aktörer som för statistik kring huspriser. Den Statistiska Centralbyrån [1] delar öppet data om prisindex i olika format (ascii/CSV, spreadsheet och i diagram). Svensk mäklarstatisik [2] får in data från majoriteten av svenska mäklare och även Hemnet [3] samlar in försäljningsstatistik. Bäst är att kontakta dem direkt beträffande vilka data de kan dela med sig av.

Det är möjligt att spara data lokalt men maskininlärning fungerar bättre ju mer data det finns. Därför bör nog data sparas i molnet och/eller i serverhallar. Stora datamängder innebär också behov av mycket datorkraft eller långa körningar för att träna ens program. Det finns flera tjänster som erbjuder både förvaring och datorkraft, t.ex. [Microsoft Azure Cloud](https://azure.microsoft.com/en-us/) och [Google Cloud](https://cloud.google.com/).

### Exempel-features [4]: 

- Boarea och trädgårdsarea.

- Antal rum, sovrum och våningar. Finns (inredd) källare?

- Ålder på fastigheten.

- Avstånd till hav/sjö/badplats, serviceställen, centrum, kommunikationer.

## 2. Databearbetning

Data kommer i en mängd olika format. Första steget i databearbetningen är således att översätta till ett lämpligt format.

Nästa steg är att visualisera rådata, d.v.s. att plotta huspriser mot alla features och features mot varandra. Detta görs för att se över kvaliteten hos datan; finns det mycket brus eller extremvärden, saknas det data (NaN)? Extremvärden går att klippa bort och brus elimineras lätt med t.ex. medelvärdesbildning, Fourier- eller Wavelet-transformer. Olika features kan också innehålla olika mycket data och noggrannheten kan behöva anpassas.

Med visualisering går det också att leta trender. Hur beror priset på alla features? Beror features på varandra? Ger vissa features ingen påverkan alls på huspriset? Om features beror på varandra går det att koppla dem och så minska antal features? För många features kan leda till överanpassning och vissa features kan behöva normaliseras om för att underlätta för maskininlärningen [5].

## 3. Linjär regression

Linjär regression är att anpassa linjära modeller mot uppsättningar features (allmänna referenser här är [5,6,7]). Grunden är att nyttja linjen *y = kx + m*. Här är *x* en feature formulerad som en vektor och *y* är vårt output, d.v.s., det förutsedda priset. Parametrarna *k* ger lutning och *m* ger skärning genom *x = 0*. Med *N* features går det att skriva

*y = W(0) + W(1) * X(1) + W(2) * X(2) + ... + W(i) * X(i) + ... + W(N) * X(N)*

där *W* är en *Weight*-matris som består av vektorerna *W(i)*. Varje *W(i)* motsvarar linjens lutning *k* för varje feature, utom *W(0)* som motsvaras av linjens konstant *m*. I matrisen *X* är varje vektor *X(i)* en feature.

Linjär regression hittar *W*-element som minimerar skillnaden mellan *y* och de verkliga priserna. Detta går att göra med en *cost function*, *J(W)*. Det är en mångdimensionell funktion som går att visualisera som en yta i ett rum uppspänt av vektorerna *W(i)*. Höga värden hos *J* innebär en dålig anpassning och låga värden innebär god anpassning.

För att minimera *J* går det att nyttja *gradient descent*. Den algoritmen "vandrar" genom *J* mot ett minimum m.h.a. derivatan *dJ/dW(i)*. Det är viktigt att välja lagom stora "vandringssteg" och ett lämpligt startvärde, annars kan algoritmen missa *J*:s minimum och oscillera kring det.

## 4. Driftsättning

Det vore praktiskt med en webbaserad tjänst där användaren fyller i features för ett hus och får en uppskattning av priset inom felmarginaler. Modellen bakom bör automatiskt uppdateras m.h.a. data från myndigheter och mäklare. Datakvaliteten bör också automatiskt granskas och inflation kompenseras för.

Det är vanligt att maskininlärningsprojekt aldrig blir implementerade för användning [8]. Därför är det nyttigt för utvecklare att lära sig grundläggande driftsättning. Det finns en mängd olika verktyg för att underlätta driftsättning, träna/uppdatera maskininlärningsprogram, skapa API:er och aktörer som säljer serverutrymme [9]. Exempel är bl.a. [Azure ML](https://azure.microsoft.com/), [AutoML](https://www.automl.org/), [IBM Watson ML](https://cloud.ibm.com/apidocs/machine-learning), [Kubeflow](https://www.kubeflow.org/) [TFX](https://www.tensorflow.org/tfx/).

## Teknologier/verktyg

- [Python](https://www.python.org/): praktiskt programmeringsspråk.

- [Numpy](https://numpy.org/): databehandling och matematik.

- [SciPy](https://www.scipy.org/): databehandling och matematik.

- [Pandas](https://pandas.pydata.org/): datanalys.

- [Matplotlib](https://matplotlib.org/): visualisering.

- [Topcat](http://www.star.bris.ac.uk/~mbt/topcat/): visualisering.

- [scikit-learn](https://scikit-learn.org/): visualisering och träna program.

- [TensorFlow](https://www.tensorflow.org/): träna program.

- [Matlab](https://matlab.mathworks.com/): databehandling och visualisering.

## Referenser

[1]: (2021-08-30) https://www.scb.se/hitta-statistik/statistik-efter-amne/boende-byggande-och-bebyggelse/fastighetspriser-och-lagfarter/fastighetspriser-och-lagfarter/

[2]: (2021-08-30) https://www.maklarstatistik.se/

[3]: (2021-08-30) https://www.hemnet.se/bostadsmarknaden

[4]: (2021-09-02) https://www.kaggle.com/camnugent/california-housing-prices

[5]: Andrew Ng, Stanford university, [Machine Learning](https://www.coursera.org/learn/machine-learning?)

[6]: Wieland Wermke, Uppsala universitet, [Statistiska analyser 7- enkel linjär regression](https://media.medfarm.uu.se/play/attachmentfile/video/4787/video.pdf)

[7]: (2021-09-01) https://en.wikipedia.org/wiki/Linear_regression

[8]: (2021-08-31) https://stackoverflow.blog/2020/10/12/how-to-put-machine-learning-models-into-production/

[9]: (2021-08-31) https://towardsdatascience.com/10-ways-to-deploy-and-serve-ai-models-to-make-predictions-336527ef00b2
