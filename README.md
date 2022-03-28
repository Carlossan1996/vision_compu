from cv2 import FONT_HERSHEY_COMPLEX_SMALL, cv2, resize

imagen = 'C:\Users\Carlos\Documents\Octavo\Robotica movil\circulo.png'
imagen1 = cv2.imread(imagen)
imagen2 = resize(imagen1, dsize=(600,600), interpolation= cv2.INTER_CUBIC)
g = cv2.cvtColor(imagen2, cv2.COLOR_BGR2GRAY)
ca = cv2.Canny(g, 10, 150)
ca = cv2.dilate(ca, None, iterations=1)
ca = cv2.erode(ca, None, iterations=1)
cn,_ = cv2.findContours(ca, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)


for c in cn:
	epsilon = 0.01*cv2.arcLength(c,True)
	m = cv2.approxPolyDP(c,epsilon,True)
	#print(len(m))
	x,y,w,h = cv2.boundingRect(m)

	if len(m)==4:
		cv2.putText(imagen2,'Cuadrado', (x-10,y),1,FONT_HERSHEY_COMPLEX_SMALL,(0,255,255),1)

	if len(m)==10:
		cv2.putText(imagen2,'Estrella', (x-10,y),1,FONT_HERSHEY_COMPLEX_SMALL,(0,255,255),1)

	if len(m)>10:
		cv2.putText(imagen2,'Circulo', (x-10,y),1,FONT_HERSHEY_COMPLEX_SMALL,(0,255,255),1)

	cv2.drawContours(imagen2, [m], 0, (0,255,255),2)
	cv2.imshow('imagen2',imagen2)
	cv2.waitKey(0)
