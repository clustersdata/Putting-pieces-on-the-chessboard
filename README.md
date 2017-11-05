# Putting-pieces-on-the-chessboard

Putting pieces on the chessboard

	在4×4的棋盘上放置8个棋,要求每一行,每一列上只能放置2个.
  
【参考程序1】

算法:8个棋子,填8次.深度为8.注意判断是否能放棋子时,两个两个为一行.


var a:array[1..8] of 0..4;

    line,bz:array[1..4] of 0..2; {line数组:每行已放多少个的计数器}
    
				 {bz数组:  每列已放多少个的计数器}
         
    total:integer;
    
procedure output; {输出}

var i:integer;

begin

     inc(total);  write(total,':   ');
     
     for i:=1 to 8  do write(a[i]);  writeln;
     
end;

function ok(dep,i:integer):boolean;

begin

 ok:=true;
 
 if dep mod 2 =0 then  {假如是某一行的第2个,其位置必定要在第1个之后}
 
    if (i<=a[dep-1])  then ok:=false;
    
 if (bz[i]=2) or(line[dep div 2]=2) then ok:=false;
 
		 {某行或某列已放满2个}
     
end;

procedure find(dep:integer);

var i:integer;

begin

     for i:=1 to 4 do begin
     
	 if ok(dep,i) then   begin
   
	    a[dep]:=i; {放在dep行i列}
      
	    inc(bz[i]); 	   {某一列记数器加1}
      
	    inc(line[dep div 2]);  {某一行记数器加1}
      
	    if dep=8 then output else find(dep+1);
      
      
	    dec(bz[i]); 	 {回溯}
      
	    dec(line[dep div 2]);
      
	    a[dep]:=0;
      
	 end;
   
     end;
     
end;

begin

     total:=0; fillchar(a,sizeof(a),0); fillchar(bz,sizeof(bz),0);
     
     find(1);
     
end.

【参考程序2】

算法:某一行的放法可能性是(1,2格),(1,3格),(1,4格)....共6种放法

const

fa:array[1..6] of array[1..2]of 1..4=((1,2),(1,3),(1,4),(2,3),(2,4),(3,4));

				{六种可能放法的行坐标}
        
var

 a:array[1..8] of 0..4;
 
 bz:array[1..4] of 0..2; {列放了多少个的记数器}
 
 total:integer;
 
procedure output;


var i:integer;

begin


     inc(total);
     
     write(total,':   ');
     
     for i:=1 to 8  do write(a[i]);
     
     writeln;
     
end;

function ok(dep,i:integer):boolean;

begin

 ok:=true;  {判断现在的放法中,相应的两列是否已放够2个}
 
 if (bz[fa[i,1]]=2) or (bz[fa[i,2]]=2) then ok:=false;
 
end;

procedure find(dep:integer);

var i:integer;

begin

     for i:=1 to 6 do begin  {共有6种可能放法}
     
	 if ok(dep,i) then   begin
   
	    a[(dep-1)*2+1]:=fa[i,1];{一次连续放置2个}
      
	    a[(dep-1)*2+2]:=fa[i,2];
      
	    inc(bz[fa[i,1]]);	     {相应的两列,记数器均加1}
      
	    inc(bz[fa[i,2]]);
      
	    if dep=4 then output else find(dep+1);
      
	    dec(bz[fa[i,1]]);	      {回溯}
      
	    dec(bz[fa[i,2]]);
      
	    a[(dep-1)*2+1]:=0;
      
	    a[(dep-1)*2+2]:=0;
      
	 end;
   
     end;
     
end;

begin

     total:=0;	fillchar(a,sizeof(a),0);    fillchar(bz,sizeof(bz),0);
     
     find(1);
     
end.
