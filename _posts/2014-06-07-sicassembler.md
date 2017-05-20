---
layout: post
title: "2-Pass SIC Assembler"
date:   2014-06-07
excerpt: "SIC 어셈블러로 포스팅에 나온 Pass1와 Pass2 알고리즘을 참고하여 개발하였다."
categories: [Seminar]
tags: [Assembler]
comments: true
---

## 2-Pass SIC Assembler  
SIC(Simplified Instructional Computer) :  실제의 기계에서 흔히 발견되는 특이성을 포함하지 않고, 가장 보편적인 하드웨어 특징과 개념을 보여줄 수 있도록 설계

## SIC 어셈블리  
어셈블리  
- 사람들이 이진 숫자로만 명령을 나타내는 것이 어려운 일이므로 이를 보다 쉽게 하기 위해 이진 숫자 대신 상징적인 단어 또는 심볼을 써서 명령을 표현  
- 각 명령의 대상이 되는 피연산자, 예를 들면 덧셈명령인 경우 더해져야 할 숫자들이 저장된 주기억장치의 주소를 숫자 대신 심볼로 나타낼 수 있게 함  

어셈블리 언어는 보통 Label, Opcode, Operand, Comment 형태로 구성됨  
- Label : 고급언어에서 문번호에 해당하는 부분  
- Opcode : 명령어 부분  
	* 지정어로 반드시 어셈블러가 인식할 수 있는 word들로 구성되어야 함  
- Operand : 명령어를 수행할 때 필요한 데이터 부분  
	* 변수나 상수에 해당하는 부분으로 프로그래머 임의로 변수명을 줄 수 있음  
	* 어셈블리에 따라서 여러 개로 구성되는 것이 보통임  
- Comment : 주석 부분  
	* Documentation 목적으로 사용하며 기계어하고는 아무런 관계가 없다  

![assembelr Image]({{ site.url }}/img/sic.png)
{: .center}

## Pass 1

{% highlight bash %}
begin
read first input line
if OPCODE = 'START' then
    begin
        save #[OPERAND] as starting address
        initialize LOCCTR to starting address
        write line to intermediate file
        read next input line
    end
else
        initialize LOCCTR to 0
while OPCODE < > 'END' do
    begin
        if this is not a comment line then
            begin
                if there is a symbol in the LABEL field then
                    begin
                        search SYMTAB for LABEL
                        if found then
                            set error flag (duplicate symbol)
                        else
                            insert (LABEL LOCCTR) into SYMTAB
                    end
                search OPTAB for OPCODE
                if found then
                    add 3 (instruotion length) to LOCCTR
                else if OPCODE = 'WORD' then
                    add 3 to LOCCTR
                else if OPCODE = 'RESW' then
                    add 3 * #[OPERAND] to LOCCTR
                else if OPCODE = 'RESB' then
                    add #[OPERAND] to LOCCTR
                else if OPCODE = 'BYTE' then
                    begin
                        find length of constant in bytes
                        add length to LOCCTR
                    end
                else
                    set error flag (invalid operation code)
            end {if not a comment}
        write line to intermediate file
        read next input line
    end {while}
write last line to intermediate file
save (LOCCTR - starting address) as program length
end {Pass 1}
{% endhighlight %}

## Pass 2

{% highlight bash %}
begin
read first input line (from intermediate file)
if OPCODE = 'START' then
    begin
        write listing line
        read next input line
    end
write Header record to object program
initialize first Text record
while OPCODE () 'END' do
    begin
        if this is not a comment line then
            begin
                search OPTAB for OPCODE
                if found then
                    begin
                        if there is a symbol in OPERAND field then
                            begin
                                search SYMTAB for OPERAND
                                if found then
                                    store symbol value as operand address
                                else
                                    begin
                                        store 0 as operand address
                                        set error flag (undefined symbol)
                                    end
                                end {if symbol}
                            else
                                store 0 as operand address
                            assemble the object code instruction
                        end {if found}
                else if OPCODE = 'BYTE' or 'WORD' then
                    convert constant to object code
                if object code will not fit into the current Text record then
                    begin
                        write Text record to object program
                        initialize new Text record
                    end
                add object code to Text record
            end {if not comment}
        write listing line
        read next input line
    end {while}
write last Text record to object program
write End record to object program
write last listing line
end {Pass 2}
{% endhighlight %}

목적 프로그램의 형식  
1. 헤더 레코드(Header record)  
열 1        H  
열 2-7     프로그램 이름  
열 8-13    목적 프로그램의 시작주소(16진수)  
열 14-19   바이트로 표시된 목적 프로그램의 길이(16진수)  

2. 텍스트 레코드(Text record)  
열 1        T  
열 2-7     레코드에 포함될 목적 코드 시작주소(16진수)  
열 8-9     바이트로 나타낸 이 레코드의 길이(16진수)  
열 10-69  16진수로 나타낸 목적 코드(목적 코드 1바이트 당 2개의 열)  

3. 엔드 레코드(End record)  
열 1        E  
열 2-7     목적 프로그램 중 첫번째로 실행될 명령어의 주소(16진수)  

패스 1 (기호 정의)
1. 프로그램 내의 모든 문에 주소 배정  
2. 모든 레이블에 배정된 주소 저장  
3. 어셈블러 지시자의 부분적 처리  
	* BYTE, RESW 등에 의하여 정의되는 데이터 영역의 길이 결정과 같은 주소배정에 영향을 주는 처리 포함  

패스 2 (명령어 번역, 목적 프로그램의 생성)  
1. 명령어 어셈블(연산자 코드 번역하고 주소 조사)  
2. BYTE, WORD 등으로 정의되는 데이터 값 생성  
3. 패스 1 동안 이루어지지 않은 지시자의 처리  
4. 목적 프로그램과 어셈블러 리스트 출력  

테이블과 변수  
- OPTAB(Operation Code Table : 연산코드 테이블)  
	* 연산 명령어 찾아 기계 코드로 번역하는 데 사용  
- SYMTAB(Symbol Table : 기호테이블)  
	* 레이블들에 배정된 주소를 저장하는 데 사용  
- LOCCTR(Location Counter : 위치계수기)  
	* 주소배정 처리하는 데 사용  
	* START 문에서 나타낸 시작주소로 초기화  
	* 각 원시 문장이 처리될 때 마다 어셈블 된 명령어나 생성된 데이터 영역의 길이가 LOCCTR에 더해지고, 원시 프로그램에서 레이블을 만날 때마다 LOCCTR의 현재 값을 레이블의 주소로 배정함  

2학년 때 시스템 프로그래밍 과목을 수강했었는데 이 때 MFC로 만든 2-pass SIC 어셈블러이다.

![assembelr Image]({{ site.url }}/img/sic-ex.png)
{: .center}

![assembelr Image]({{ site.url }}/img/sic-ex1.png)
{: .center}


[2-Pass SIC Assembler source code]

[2-Pass SIC Assembler source code]: https://github.com/yunsangq/2-Pass-SIC-Assembler
