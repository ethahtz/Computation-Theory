# Computation-Theory
Etha Hua's Turing Machine codes for phil170, computation theory. 


------3a------
Let L be the language {σσ|σ ε {a,b}*}. 3a is a Turing machine that begins by scanning the leftmost symbol of a string of {a,b}* on an otherwise blank tape and halts scanning a '1' on an otherwise blank tape if the initial string is a sentence of L and halts scanning a '0' on an otherwise blank tape if the initial string is not a sentence of L.  (I.e., it is a TM that solves the decision problem for L.)   

Overall implementation plan for 3a: 
1. Translate all a’s and b’s into x’s and y’s in order to find the midpoint of the string, and add an “s” indicating the midpoint of the string.
2. Compare the two strings by checking whether the first symbol is the same, then the second and so on. This step is done by erasing the nth symbol on the left string and carry the information by entering a corresponding state that preserves that information, and when it goes to the right and reaches the symbol after the “s”s separating the two strings, check whether the symbol matches that state or not, if so, rewrite the nth symbol of the right string to “s” and go to the left, if not, erase everything and write 0.
3. In the end, if the right-checking state reaches a blank symbol immediately after the s’s, it means the two strings have been completely checked to be the same, then erase everything and write 1.

------4a------
4a is a Turing machine that does binary multiplication. It begins by scanning the leftmost symbol of a string comprising a binary numeral, followed by an 's', followed by another binary numeral (on an otherwise blank tape),  and  halts  scanning  the  leftmost  symbol  of  the  binary product of the two numbers represented by the initial binary numerals (on an otherwise blank tape).  

Overall implementation plan for 4a: 

Design: the string will initially be ###nsm### which represents n times m,
I will use another letter “q” to separate the string into ###rqnsm### where “r” will be the result of the intended multiplication. Generally, the machine will do a binary addition by adding n to r, and then every time the addition is completed, decrement m by 1. When m is decremented to zero, erase everything except the r string, which is the result. 

Steps: first of all, copy the string n into r, and separating them with “q”. This step is done by translating 0’s into x’s and 1’s into y’s. First of all, write a “q” to the leftmost symbol. Then go to the rightmost symbol of n by stopping when reaches “s”, and translate that symbol into “x” or “y” and carry the information by entering a corresponding state and go to the left, go through “q” and write the corresponding 1 or 0. Repeat the step until the string is copied. To check the string is copied: when the copy state going to find the next symbol that is neither x’s or y’s is a “q” rather than a 0 or a 1, it will recognize that the string is copied. Go to translate the x’s and y’s back to 1’s and 0’s and then when reaches “s” go to the next state. 

When reaches “s”, decrement m by 1. (That is, if the last digit is a 1, change it into a zero; if the last digit is not a 1, change it to a 1, and go to the left: change all 0’s on the left to 1 until it reaches a 1, if so, change it to a zero.) (p.s. there is a zero checker after every decrement: since the order is first do the addition and then do the decrement, when the zero checker check m to be 0, it will go to erase everything step.)

And then go to the left to do an addition r+n. From the least digit of n add onto the corresponding digit of r. Every time read a digit from n, translate it into x or y as marked to have read it. If non-carry: just write the corresponding x or y on that digit of r; if carry, write the corresponding x or y on that digit of r, and then proceed to the left to discharge the carried value: if meets a 1, rewrite it into a 0 and carry on; until meets a 0 or a #, write a 1. When all digits from n is marked, the addition is completed, go to the left most and start to translate the x’s and y’s back into 1’s and 0’s and go to s and start to decrement m.


------5a------
Let L be the (type 3) language on {a,b} consisting of all strings that begin with an 'a' and have exactly one 'b' in them. 5a is a Turing machine that generates L in the recommended sequence: ab, aab, aba, aaab, aaba, abaa, aaaab, aaaba, ....

Overall implementation plan for 5a:

I devised a recursive way for the generator to proceed: 
1.Starts with the string “xy” on an otherwise blank tape. (actually the x and y are just two stipulated starting states to write on a completely blank tape)

2.To the right, when it reaches a blank symbol, write a comma, and then go to the leftmost symbol (either leftmost with regard to it is next to a blank symbol or with regard to it is next to the former comma symbol)

3.Translate the first symbol (x into a, y into b) and copy it in form of x or y after the rightmost comma. Copy the entire string. The machine will recognize that the former string is completely copied when it reaches an “a” or a “b” right to the left of the comma.

4.The next step is to modify the copied string: two situations: 
1) If the head of the new tape is in form of “xy….” Rewrite the “y” into an “x”, and go beyond the rightmost symbol and write a “y”, then a comma. Then go to stage 3.
2) If the head of the new tape is otherwise, go to the right until reaches a “y”, rewrite the “y” into an “x” and write a “y” to the left of that “x”, and go to the rightmost spot, add a comma. Then go to stage 3.
