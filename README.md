# Take-it-easy
## Descripción
![Descripcion](https://github.com/m0riart3/Take-it-easy/blob/main/descripcion.PNG)

## Solución al RSA
Al abrir el .zip vemos que contiene otro zip con clave llamado TRYME.zip y un archivo .txt llamado getkey.txt, el cual contiene el modulo, el texto cifrado y el exponente de un RSA, usando las propiedades basicas del RSA obtenemos la clave del TRYME.zip con el siguiente script
```
from Crypto.Util.number import long_to_bytes
from gmpy2 import iroot
n = 147310848610710067833452759772211595299756697892124273309283511558003008852730467644332450478086759935097628
3365307356071689041296997522660567218794518405064814437453405099353334118358375484853620307931409724348733940725
78851922470507387225635362369992377666988296887264210876834248525673247346510754984183551
ct = 43472086389850415096247084780348896011812363316852707174406536413629129
e = 3
print(long_to_bytes(iroot(ct,3)[0]))
```
dandonos como resultado
```
m0riart3㉿kali)-[~/Desktop/dark/takeitEasy]
└─$ python3 script1.py 
b'Ju5t_@_K3Y'
```
descomprimir el .zip vemos que contiene un archivo llamado cipher.txt con lo que suponemos que es la flag encriptada y un script en python con el algoritmo de cifrado,
el script es el siguiente
```
#!/usr/bin/env python3

from struct import pack, unpack
flag = b'darkCON{XXXXXXXXXXXXXXXXXXX}'

def Tup_Int(chunk):
	return unpack("I",chunk)[0]

chunks = [flag[i*4:(i+1)*4] for i in range(len(flag)//4)]
ciphertext = ""

f = open('cipher.txt','w')
for i in range(len(chunks) - 2):
	block = pack("I", Tup_Int(chunks[i]) ^ Tup_Int(chunks[i+2]))
	ciphertext = 'B' + str(i) + ' : ' + str(block) + '\n'
	f.write(ciphertext)
  ```
  tras unos cuantos cambios a las lineas del script original, sabiendo que la cabecera de la flag es siempre la misma podemos obtener el resto de la flag haciendo la operacion xor de la flag encriptada con la cabecera de la flag para ir obteniendo la flag, usando este script obtenemos la flag
  ```
  #!/usr/bin/env python3

from struct import pack, unpack
chunks = [b'dark',b'CON{']

def Tup_Int(chunk):
	return unpack("I",chunk)[0]

ciphertext = [b'\nQ&4',b"\x17'\x0e\x0f",b'1X5\r', b'072E', b'\x18\x00\x15/']
for i in range(len(ciphertext)):

	cifrado = Tup_Int(ciphertext[i]) ^ Tup_Int(chunks[i])
	chunks.append(pack("I",cifrado))

print(str(chunks))
```
dandonos por salida la flag 
```
m0riart3㉿kali)-[~/Desktop/dark/sol]
└─$ python3 chall.py 
[b'dark', b'CON{', b'n0T_', b'Th@t', b'_haR', b'd_r1', b'Ght}']
```

