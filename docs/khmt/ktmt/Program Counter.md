Usually, the PC is incremented after fetching an instruction, and holds the memory address of ("points to") the next instruction that would be executed. Là một bộ phận trong [[CPU]], thuộc nhóm các [[Register]]s
![[img/Pasted image 20250527094924.png]]
Nền tảng của PC là **counter**: a sequential chip whose state is an integer number that increments every time unit, effecting the function $out(t) = out(t - 1) + c$, where c is typically 1.
![[img/Pasted image 20250527094939.png]]