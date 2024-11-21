find x s.t x = 2 (mod 3 )
		x = 3 (mod 5)
		x = 2 (mod 7)

	mod 3     mod 5     mod 7
x' =  5.7         +   3.7        +    5.3 
	^                ^                  ^  	
controlled remainders for each modulos, independently

x' yield non-zero remainders, though not accurate, but we can fix it 
5.7 = 2 (mod 3) => correct 
3.7 = 1 (mod 5) => not correct  => 2x  3.7 = 2 (mod 5)
5.3 = 1 (mod 7) => not correct => 2x   5.3 =  2 (mod 7)

-> Sum above 
**Main concept**: divide x into 3 parts, s.t we can fix each part indepently according to the remainders.




# Chinese remainder theorem
--- 
# Refererences 




2022 08 06 13:49
#idea [[Atomic Ideas/maths]] [[number theory]]