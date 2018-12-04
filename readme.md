# Analisis computacional-cognitivo-conductual del juego de cartas Truco Argentino.

�Podemos crear una IA para el truco?

**Resumen:** El Truco es un juego de cartas Argentino popularmente jugado en el todo el pa�s y el cono sur. 
Es un juego de estrategia competitivo basado en turnos, de estados finitos e informaci�n incompleta, lo c�al quiere decir que los jugadores basaran sus estrategias en base a especulaciones en cuanto a las cartas de los demas.
En los �ltimos tiempos, han existido numerosos articulos cientificos implementando modelos de aprendizaje reforzado en juegos de dichas caracteristicas. No obstante, aun no hay ningun trabajo que haya analizado este particular juego Argentino.
El pr�posito de este proyecto es analizar este juego desde un aspecto computacional para poder, luego, modelar un agente de aprendizaje reforzado.
Para dicho fin, primero vamos a hacer una revisi�n de los trabajos cientificos en juegos similares (como el poker), luego conceptualizar algunos terminos clave en funci�n del comportamiento y estilo cognitivo de jugadores de truco y finalmente, proponemos a la comunidad el involucramiento en el desarrollo de un agente de aprendizaje reforzado para resolver este problema.

## Facilitador

Patricio J. Gerpe

## Preliminares
### Revisi�n de literatura

En tanto no se han encontrado art�culos cientificos en respecto al juego de cartas 'Truco' si existe art�culos cientificos que abordan juegos de cartas de informaci�n imperfecta tal como el Poker.
Ejemplos incluyen:

* Heinrich, J., & Silver, D. (2016). Deep reinforcement learning from self-play in imperfect-information games. arXiv preprint arXiv:1603.01121. https://arxiv.org/pdf/1603.01121.pdf
* Dahl, F. A. (2001, September). A reinforcement learning algorithm applied to simplified two-player Texas Hold�em poker. In European Conference on Machine Learning (pp. 85-96). Springer, Berlin, Heidelberg.
* Te�filo, L. F., Passos, N., Reis, L. P., & Cardoso, H. L. (2012). Adapting strategies to opponent models in incomplete information games: a reinforcement learning approach for poker. In Autonomous and Intelligent Systems (pp. 220-227). Springer, Berlin, Heidelberg.
* Erev, I., & Barron, G. (2005). On adaptation, maximization, and reinforcement learning among cognitive strategies. Psychological review, 112(4), 912. https://www.researchgate.net/profile/Ido_Erev/publication/7505648_On_Adaptation_Maximization_and_Reinforcement_Learning_Among_Cognitive_Strategies/links/00b49524696d50e515000000.pdf
* Szita, I. (2012). Reinforcement learning in games. In Reinforcement Learning (pp. 539-577). Springer, Berlin, Heidelberg.
* https://arxiv.org/pdf/1603.01121.pdf


### Conceptos Clave

* Modelado del oponente:
Dado que no podemos saber qu� cartas tiene nuestro oponente es necesario poder modelar el estilo del juego del mismo.

* Equilibrio de Nash:

* Valor esperado:
El valor esperado es el puntaje que podemos ganar al tomar una acci�n (Ejemplo: Ganar envido vs Perder envido) multiplicado por la probabilidad de ocurrencia de dicho desenlance.


### Modelado de perfiles de jugadores

Perfiles de Estrategia (Aspecto Conductual)	Descripcion	Frecuencia de Mentira	Tendencia a Cantar con	Tendencia a Aceptar Apuesta	Tendencia a Aumentar apuesta	Eficaz contra	Debil contra
Creativo	Cambia sus t�cticas con frecuencia, crea nuevas jugadas espontaneamente.	Impredecible	Impredecible	Impredecible	Impredecible	Todos	Ninguno
Temerario	Miente con mucha frecuencia, y suele arriesgar muchos puntos teniendo cartas pobres. 	Muy alta	Cartas malas	Muy alta	Alta	Miedoso	Tradicional
Tradicional	Miente seguido con cartas medias y malas, en alguna ocasi�n hace un gran aumento de apuestas con cartas malas.	Alta	Cartas medias / malas	Alta	Media	Temerario	Pescador
Pescador	Canta cuando tiene cartas buenas y medias, aunque cada tanto realiza una mentira teniendo cartas malas.	Media	Cartas buenas / medias	Media	Media	Tradicional	Conservador
Conservador	Canta cuando tiene buenas cartas. Suele decir que no quiere cuando tiene cartas malas.	Baja	Cartas excelentes / buenas	Media	Baja	Pescador	Tradicional
Miedoso	Solo canta cuando tiene cartas excelentes. Pr�cticamente renuncia a todos los aumentos de apuesta.	Nula	Cartas excelentes	Baja	Muy baja	Ninguno	Todos


Perfiles de Estrategia (Aspecto Conductual)	Descripcion	Frecuencia de Mentira	Tendencia a Cantar con	Tendencia a Aceptar Apuesta	Tendencia a Aumentar apuesta	Eficaz contra	Debil contra
Creativo	Cambia sus t�cticas con frecuencia, crea nuevas jugadas espontaneamente.	Impredecible	Impredecible	Impredecible	Impredecible	Todos	Ninguno
Temerario	Miente con mucha frecuencia, y suele arriesgar muchos puntos teniendo cartas pobres. 	Muy alta	Cartas malas	Muy alta	Alta	Miedoso	Tradicional
Tradicional	Miente seguido con cartas medias y malas, en alguna ocasi�n hace un gran aumento de apuestas con cartas malas.	Alta	Cartas medias / malas	Alta	Media	Temerario	Pescador
Pescador	Canta cuando tiene cartas buenas y medias, aunque cada tanto realiza una mentira teniendo cartas malas.	Media	Cartas buenas / medias	Media	Media	Tradicional	Conservador
Conservador	Canta cuando tiene buenas cartas. Suele decir que no quiere cuando tiene cartas malas.	Baja	Cartas excelentes / buenas	Media	Baja	Pescador	Tradicional
Miedoso	Solo canta cuando tiene cartas excelentes. Pr�cticamente renuncia a todos los aumentos de apuesta.	Nula	Cartas excelentes	Baja	Muy baja	Ninguno	Todos


SEG�N SU EXPERTISE	Nivel de Atenci�n y Concentraci�n	Experiencia
Experto	Si bien puede no aparentarlo siempre esta muy concentrado y atento en el juego. Tiene una gama muy alta de t�cticas. Un nivel de actuaci�n muy alto, sabe mentir. Cambia con frecuencia su repertorio de t�cticas y perfiles de estrategia en funci�n del juego.	Varios a�os
Experimentado		Varios a�os
Medio	Conoce las t�cticas y se�as, sin embargo a veces se lo ve distraido y desentendido del juego.	Algunos a�os
B�sico	Esta empezando a entender el juego, empieza a incorporar sus propias t�cticas y perfiles de estrategia.	Menos de un a�o
Inicial	No entiende ni conoce las t�cticas tipicas, no suele indetificar mentiras. Puede estar distraido. No conoce las se�as y generalmente precisa ayuda externa.	Primeras 20 partidas
		
		
		
SEG�N SU FORMA DE PENSAMIENTO (Aspecto Cognitivo)		
Convinado		
Racional		
Intuitvo		


		Ideal cuando	Efectivo cuando	Riesgo			
Ir a la pesca	Se tiene tanto o truco medio o alto, no se canta y se espera a que el contrincante cante para doblar la apuesta o aceptar.				Se tiene	No se canta	Dobla Apuesta
Envido de Cobertura	Se tiene buen tanto pero malas cartas para el  truco, el oponente canta truco en la primera mano, y se dice "el envido esta primero" para postergar el truco y obtener puntos del tanto antes de retirarse.						
Mentira del tanto	No se tienen buen tanto, se canta o se acepta y al momento de decir los puntos se miente por un nro m�s alto, esperando a que se termine la partida y el rival haya olvidado pedir que se muestren los tantos.	Jugamos con contrincantes muy distraidos	Nuestro oponente se olvido de solicitar que se muestren los tantos al finalizar la partida.				
Trampa del truco	Se tiene muy buenas cartas de envio y cartas medias/buenas de truco, en la primera mano cantamos truco esperando que nuestro oponente nos diga "el envido esta primero" de manera tal que podamos doblar la apuesta.				Se tiene	Canta truco	Dobla Apuesta
Achicar	No se tiene buenas cartas (tanto o truco), el oponente canta primero y  dobla la apuesta buscando que el oponente la rechace.				No se tiene	Dobla apuesta	No quiero
Hacerlos entrar	Se tiene buenas cartas, el contricante canta primero y se dobla la apuesta para buscar  m�s puntos.				Se tiene	Dobla apuesta	Se quiere
Se�as falsas	El oponente nos mira y hacemos una se�a falsa que simulamente va dirigida a nuestro compa�ero para confundir al rival.						
Se�as personalizadas	Junto a tu compa�ero se crean nuevas se�as que unicamente ustedes conocen de manera tal de confundir al rival.						
Jugar callado	No se tiene buenas cartas, y no se canta nada esperando que pase desapercibido para el oponente y tampoco lo haga.						
Hacersela	Apurar a un oponente para que juege cuando no es su turno, de manera que tire y queme su carta.						
El error	Estar hablando y simular que se te escap� la palabra truco o envido de manera que los oponentes quieran tomar provecho, acepten y/o doblen la apuesta esperando que nosotros no tengamos nada.						
Falta envido de Cobertura	El oponente esta a pocos puntos de ganar (menos de 3) , vos no tenes muchos tantos, te canta tanto o te dobla una apuesta. Para minimizar riesgos le cantas falta envido para reducir los puntos en juego.				No se tiene	Se canta	Minimiza Apuesta
Apriete	Nuestro oponente esta a pocos puntos de perder, tenemos cartas y sabiendo que s� o s� debe aceptar, cantamos tanto o truco para acepte y pierda.				Se tiene	Se canta	Se quiere




* Indice de agresividad:
* Indice de variabilidad:
