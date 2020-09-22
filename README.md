<div align="center">

## Enigma Machine


</div>

### Description

The Enigma machine, created by the German inventor Arthur Scherbius, was used as Germany's system of encryption before and during World War II. This worksheet emulates a version of the Enigma machine.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Abdulaziz Alfoudari](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/abdulaziz-alfoudari.md)
**Level**          |Advanced
**User Rating**    |3.7 (11 globes from 3 users)
**Compatibility**  |VB 4\.0 \(32\-bit\), VB 5\.0, VB 6\.0
**Category**       |[Encryption](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/encryption__1-48.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/abdulaziz-alfoudari-enigma-machine__1-31269/archive/master.zip)





### Source Code

<p align="center"><font face="Verdana" size="5">Visit us at: </font>
<font face="Verdana" size="2">
<a style="font-family: Verdana; color: #800000; text-decoration: none; font-size: 8pt" title="VBParadise.com" href="http://www.vbparadise.com">
http://www.vbparadise.com</a></font><p align="center"><b>
<font face="Verdana" size="4">The Enigma Machine</font></b><p align="left">
<font face="Verdana" size="2"><br>
<b>Introduction </b><br>
The Enigma machine, created by the German inventor Arthur Scherbius, was used as
Germany's system of encryption before and during World War II. This worksheet
emulates a version of the Enigma machine. <br>
<br>
The three main components of the Enigma machine were a plugboard, three
scramblers, and a reflector. <br>
<br>
The plugboard was used to swap six pairs of letters before they entered the guts
of the machine. For example, if I swapped letters 'a' and 'b', and then typed in
the letter 'b', it would follow the encryption path originally associated with
the letter 'a'. <br>
<br>
The scrambler, a thick rubber disk filled with wires, was the most important
part of the machine. Each scrambler was essentially a monoalphabetic cipher,
which encrypted one plaintext letter to a ciphertext letter. Each letter that
was typed into the keyboard of the Enigma, passed through three scramblers, so
was encrypted three times. The most important feature of the scramblers was that
after each letter was encrypted, they would change their orientation to create a
new cipher in the following manner. The first scrambler would rotate 1/26 of a
revolution, and after one whole revolution, it would turn the second scrambler
1/26 of a revolution. After one whole revolution of the second scrambler, it
would eventually turn the third scrambler one notch. This is a similar to a car
odometer, the rotor representing single km has to rotates once completely before
the rotor representing 10's of km rotates by one unit. Because of this rotation
of three scramblers, the cipher text obtained after typing in the same letter
consecutively would only repeat itself after the 17,576th letter. <br>
<br>
The last part of the Enigma machine was the reflector, which was a little bit
like the scramblers, but it did not rotate, and the wires entered and exited
through the same side. So if a letter 'a' goes into the reflector, it might come
out as a letter 's', and then get sent back through the three scramblers in
reverse order, through the plugboard, and finally show up as the encrypted
letter. The reflector allows users to use the Enigma in the same way for both
encrypting and decrypting messages. <br>
<br>
Because of the changing nature of the machine, sender and receiver must agree on
a set of keys, or initial conditions, before communicating with the Enigma. On
the actual Enigma machine, the three scramblers could be interchanged, meaning
that there were 6 different orders that they could be positioned in. (123, 132,
213, 231, 312, 321) Also, each scrambler could be rotated to any one of 26
initial orientations, yielding 26*26*26 = 17,576 different settings. Also the
number of ways of swapping six pairs of letters out of 26 is 100,391,791,500! So
the total number of keys available is 6*17,576*100,391,791,500 =
10,586,916,764,424,000!!<br>
<br>
<b>Code</b><br>
<b><font color="#FF0000">> restart; </font></b></font><p align="left">
<font face="Verdana" size="2"><br>
Given a seed, this will create a scrambler with random internal wiring. </font>
<p align="left"><font face="Verdana" size="2"><br>
<b><font color="#FF0000">> makeScrambler := proc(seed)<br>
local numGen, i, j, num, scrambler;<br>
scrambler := [];<br>
randomize(seed);<br>
numGen := rand(1..26);<br>
for i from 1 to 26 do<br>
num := numGen();<br>
for j while member(num, scrambler) do<br>
num := numGen();<br>
end do;<br>
scrambler := [op(scrambler),num];<br>
end do;<br>
end proc: <br>
Given a seed, this will create a reflector with random internal wiring. <br>
> makeReflector := proc(seed)<br>
local num, pos, i, j, reflector, numGen, temp;<br>
reflector := table();<br>
randomize(seed);<br>
numGen := rand(1..26);<br>
temp := [ entries(reflector) ];<br>
for i while nops(temp) <> 26 do<br>
pos := numGen();<br>
for j while member([pos],temp) do<br>
pos := numGen();<br>
end do;<br>
num := numGen();<br>
for j while member([num],temp) or num = pos do<br>
num := numGen();<br>
end do;<br>
reflector[pos] := num;<br>
reflector[num] := pos;<br>
temp := [ entries(reflector) ]; <br>
end do;<br>
for i from 1 to 26 do<br>
temp[i] := temp[i][1];<br>
end do;<br>
temp;<br>
end proc: </font></b></font><p align="left"><font face="Verdana" size="2"><br>
This will rotate a scrambler passed in by list, by making the n'th letter in the
list the first letter. </font><p align="left"><font face="Verdana" size="2"><br>
<b><font color="#FF0000">Ex: list := [1, 2, 3, 4, 5]; <br>
list := rotate(list, 3); <br>
list; <br>
[3, 4, 5, 1, 2] <br>
> rotate := proc(list, n)<br>
local i, toReturn, rotateBy;<br>
toReturn := [];<br>
rotateBy := n - 1;<br>
for i from 1 to 26 do<br>
rotateBy := rotateBy + 1;<br>
toReturn := [op(toReturn),list[((rotateBy - 1)mod 26) + 1]];<br>
end do;<br>
end proc: </font></b></font><p align="left"><font face="Verdana" size="2"><br>
This will create three scramblers, position them in the order given by
scramOrder, arrange them according to orientation, and create a plugboard
according to plugSettings. It will then encrypt or decrypt (the processes are
the same) message and return the result. </font><p align="left">
<font face="Verdana" size="2"><br>
<b><font color="#FF0000">> enigma := proc(message, scramOrder, orientation,
plugSettings)<br>
local i, scramblers, scram1, scram2, scram3, letter1, letter2, letter3,
plainText,<br>
pair1, pair2, pair3, pair4, pair5, pair6, plugboard, reflector, cipherText,
toReturn,<br>
plugIndex, plugEntry;<br>
plainText := StringTools[LowerCase](message);<br>
plainText := convert(plainText, bytes);<br>
scramblers := [ makeScrambler(456), makeScrambler(504), makeScrambler(607) ];<br>
scram1 := scramblers[scramOrder[1]];<br>
scram2 := scramblers[scramOrder[2]];<br>
scram3 := scramblers[scramOrder[3]];<br>
letter1 := convert(StringTools[LowerCase](orientation[1]),bytes)[1] - 96;<br>
letter2 := convert(StringTools[LowerCase](orientation[2]),bytes)[1] - 96;<br>
letter3 := convert(StringTools[LowerCase](orientation[3]),bytes)[1] - 96;<br>
scram1 := rotate(scram1, letter1);<br>
scram2 := rotate(scram2, letter2);<br>
scram3 := rotate(scram3, letter3);<br>
pair1 := convert(StringTools[LowerCase](plugSettings[1]),bytes);<br>
pair2 := convert(StringTools[LowerCase](plugSettings[2]),bytes);<br>
pair3 := convert(StringTools[LowerCase](plugSettings[3]),bytes);<br>
pair4 := convert(StringTools[LowerCase](plugSettings[4]),bytes);<br>
pair5 := convert(StringTools[LowerCase](plugSettings[5]),bytes);<br>
pair6 := convert(StringTools[LowerCase](plugSettings[6]),bytes);<br>
plugboard := table([(pair1[1] - 96) = pair1[2] - 96, (pair2[1] - 96) = pair2[2]
- 96,<br>
(pair3[1] - 96) = pair3[2] - 96, (pair4[1] - 96) = pair4[2] - 96,<br>
(pair5[1] - 96) = pair5[2] - 96, (pair6[1] - 96) = pair6[2] - 96]);<br>
plugIndex := [indices(plugboard)];<br>
plugEntry := [entries(plugboard)];<br>
for i from 1 to 6 do<br>
plugIndex[i] := plugIndex[i][1];<br>
plugEntry[i] := plugEntry[i][1];<br>
end do; <br>
reflector := makeReflector(978);<br>
cipherText := encode(plainText, plugIndex, plugEntry, scram1, scram2, scram3,
reflector);<br>
toReturn := convert(cipherText, bytes);<br>
end proc: <br>
> encode := proc(plainText, plugIndex, plugEntry, scrm1, scrm2, scrm3,
reflector)<br>
local i, scram1, scram2, scram3, pos1, pos2, pos3, letter, response;<br>
scram1 := scrm1; scram2 := scrm2; scram3 := scrm3;<br>
pos1 := 1; pos2 := 1; pos3 := 1;<br>
response := [];<br>
for i from 1 to nops(plainText) do<br>
letter := plainText[i] - 96;<br>
if letter > 0 and letter < 27 then<br>
if member(letter, plugIndex, 'x') then<br>
letter := plugEntry[x];<br>
elif member(letter, plugEntry, 'y') then<br>
letter := plugIndex[y];<br>
end if;<br>
letter := scram1[letter];<br>
letter := scram2[letter];<br>
letter := scram3[letter];<br>
letter := reflector[letter];<br>
member(letter, scram3, 'letter');<br>
member(letter, scram2, 'letter');<br>
member(letter, scram1, 'letter');<br>
if member(letter, plugEntry, 'z') then<br>
letter := plugIndex[z];<br>
elif member(letter, plugIndex, 'w') then<br>
letter := plugEntry[w];<br>
end if;<br>
end if;<br>
scram1 := rotate(scram1, 2);<br>
pos1 := pos1 + 1;<br>
if pos1 > 26 then<br>
pos1 := 1;<br>
scram2 := rotate(scram2, 2);<br>
pos2 := pos2 + 1;<br>
if pos2 > 26 then<br>
pos2 := 1;<br>
scram3 := rotate(scram3, 2);<br>
pos3 := pos3 + 1;<br>
if pos3 > 26 then<br>
pos3 := 1;<br>
end if;<br>
end if;<br>
end if;<br>
response := [op(response), letter + 96];<br>
end do; <br>
end proc: </font></b><br>
 </font><p align="left"><font face="Verdana" size="2"><br>
<b>Results </b><br>
<b><font color="#FF0000">> message := "There is one more feature of Scherbius's
design, known as the ring, which has not yet been mentioned. Although the ring
does have some effect on encryption, it is the least significant part of the
whole Enigma machine."; </font></b></font><p align="left" style="margin-top: 0">
<img border="0" src="http://www.mapleapps.com/categories/mathematics/Cryptography/html/images/enigma/enigma1.gif" width="934" height="20"><font face="Verdana" size="2">
</font><p align="left" style="margin-top: 0"><font face="Verdana" size="2">
<img border="0" src="http://www.mapleapps.com/categories/mathematics/Cryptography/html/images/enigma/enigma2.gif" width="479" height="20"></font><p align="left">
<font face="Verdana" size="2"><br>
<br>
<b><font color="#FF0000">> code := enigma(message, [2,3,1], [c,b,t],[tg,yh,rf,ep,av,bn]);
</font></b></font><p align="left" style="margin-top: 0">
<img border="0" src="http://www.mapleapps.com/categories/mathematics/Cryptography/html/images/enigma/enigma3.gif" width="935" height="20"><font face="Verdana" size="2">
</font><p align="left" style="margin-top: 0"><font face="Verdana" size="2">
<img border="0" src="http://www.mapleapps.com/categories/mathematics/Cryptography/html/images/enigma/enigma4.gif" width="513" height="20"></font><p align="left" style="margin-top: 0">
<font face="Verdana" size="2"><br>
<br>
<br>
<b><font color="#FF0000">> result := enigma(code, [2,3,1], [c,b,t],[tg,yh,rf,ep,av,bn]);
</font></b></font><p align="left" style="margin-top: 0">
<img border="0" src="http://www.mapleapps.com/categories/mathematics/Cryptography/html/images/enigma/enigma5.gif" width="934" height="20"><font face="Verdana" size="2">
</font><p align="left" style="margin-top: 0"><font face="Verdana" size="2">
<img border="0" src="http://www.mapleapps.com/categories/mathematics/Cryptography/html/images/enigma/enigma6.gif" width="447" height="20"></font><p align="left" style="margin-top: 0"> <p align="left" style="margin-top: 0; margin-bottom: 0">
<font face="Verdana" size="2"><br>
The actual Enigma machine did not have upper and lower case letters, and did not
have any spaces or punctuation, so this worksheet will always output lower case
letters and ignore spaces and puntuation. <br>
If we try to decrypt the message with slightly different initial conditions, the
result will be gibberish. </font><p align="left" style="margin-top: 0"> <p align="left" style="margin-top: 0">
<font face="Verdana" size="2"><br>
<b><font color="#FF0000">> result2 := enigma(code, [2,3,1], [a,b,t], [tg,yh,rf,ep,av,bn]);
</font></b></font><p align="left" style="margin-top: 0">
<img border="0" src="http://www.mapleapps.com/categories/mathematics/Cryptography/html/images/enigma/enigma7.gif" width="935" height="20"><font face="Verdana" size="2">
</font><p align="left" style="margin-top: 0"><font face="Verdana" size="2">
<img border="0" src="http://www.mapleapps.com/categories/mathematics/Cryptography/html/images/enigma/enigma8.gif" width="515" height="20"></font><p align="left" style="margin-top: 0"> <p align="center" style="margin-top: 0">
<font face="Verdana" size="2"><br>
<br>
<b>Note:</b> This article was taken from:<br>
Sylvain Muise<br>
smuise@student.math.uwaterloo.ca</font>

