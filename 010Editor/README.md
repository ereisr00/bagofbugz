010Editor is a hex-editor that includes a sophisticated template and scripting engine.
One of the leading advantages of 010Editor is that it allows users and developers to write code that parses and presents an elaborate definition of a file and its structure in an easily digestable fashion (colors and windows). 
In the case of templates, the code is manifested in a .bt file that contains C-like code that is compiled and executed by the scripting engine when a template is loaded.

Just as have many other people, I've been using 010Editor for a while. This software is a leader in its (admitedly) specific market, and it's hard to find a good alternative.
I was curious on how the 010Editor scripting engine was implemented, so a few days ago, I decided to audit the security of the scripting engine.

This folder contains some of the results:

memcpy_rce.bt - an example POC that spawns gnome-calculator on linux when running a malicious template
strcat_heap_overflow.bt - an example POC that causes a heap overflow in the template engine Strcat implementation when running a maicious template
null_deref.bt - an example POC that causes an access violation in the template engine variable initialization implementation when running a maicious template

memcpy_rce.bt

```Assembly

/*
 *  Internal implementation of function Memcpy in the template engine
 *  This function contains a bug that could cause a heap underflow/overflow
 */
.text:00000000004E3D70 RCompile::Memcpy(_node *, RList *)
                                       ...
.text:00000000004E3E07                 mov     edi, [rdx+8]    // dest_offset
.text:00000000004E3E0A                 mov     rdx, [rax+20h]
.text:00000000004E3E0E                 mov     rax, [rax+10h]
.text:00000000004E3E12                 mov     ecx, [r9+4]
.text:00000000004E3E16                 mov     esi, [rdx+8]    // src_offset
.text:00000000004E3E19                 mov     edx, [r8+4]
.text:00000000004E3E1D                 mov     eax, [rax+8]
.text:00000000004E3E20                 sub     ecx, edi        // dest_len -= dest_offset 1) no check on dest_offset for negative value
.text:00000000004E3E22                 sub     edx, esi        // src_len -= src_offset   2) no check on src_offset for negative value
.text:00000000004E3E24                 cmp     ecx, edx        // if (dest_len < src_len)
.text:00000000004E3E26                 cmovle  edx, ecx        //   copy_length = dest_len
.text:00000000004E3E29                 cmp     edx, eax        
.text:00000000004E3E2B                 cmovg   edx, eax        
.text:00000000004E3E2E                 test    edx, edx        
.text:00000000004E3E30                 jle     short loc_4E3E48 
.text:00000000004E3E32                 movsxd  rdi, edi        
.text:00000000004E3E35                 movsxd  rsi, esi        
.text:00000000004E3E38                 add     rdi, [r9+8]     // dest += dest_offset     3) passing dest += dest_offset could lead to underflow
.text:00000000004E3E3C                 add     rsi, [r8+8]     // src += src_offset       4) passing src += src_offset could lead to underflow
.text:00000000004E3E40                 movsxd  rdx, edx        // n
.text:00000000004E3E43                 call    _memcpy         ; Call Procedure
...


```

strcat_heap_overflow.bt

```C

/*
 * This function adds two variables of unknown type, including strings
 * The function contains a bug that could lead to a heap overflow
 */
__int64 RVariant::Add@(RVariant *this, RVariant *str_2, double a3)
{
  int len_1; 
  int len_2; 
  int total_len;
  char *src;

  else if ( this->type <= 0x67u )
  {
    switch ( this->type )
    {
      case 0:
      case 1:
      ...
      case VAR_TYPE_STRING:
        len_1 = RVariant::Strlen(this);
        len_2 = RVariant::Strlen(str_2);
        if ( len_2 == -1 || len_1 == -1 )
          goto finish;
        // 1) total_len is signed: the addition of len_1 + len_2 can result in a signed integer
        total_len = len_1 + len_2;
        // 2) This check does not take into account the issue above
        if ( len_1 + len_2 >= this->length )
        {
          ...
          RVariant::InitArray(this, 0x64, total_len + 1);
          strncpy(this->data, src, len_1);
          strncpy(this->data[len_1], str_2, len_2_);
          (this->data[total_len] = 0;
          ...
          goto XXX;
        }
        // 3) Due to issues 1 and 2, the heap can be overflown in this strcpy
        strncpy(this->data[len_1], str_2, len_2);
        this->data[total_len] = 0;
        break;
        ...

```

010 Editor version 9.0.1 is affected by these vulnerabilities.
The bugs were reported and fixed in version 9.0.2.

the following CVE IDs were assigned for the vulnerabilities:

memcpy_rce.bt: CVE-2019-12551
null_deref.bt: CVE-2019-12552
strcat_heap_overflow.bt: CVE-2019-12553
SubStr.bt: CVE-2019-12555
WSubStr.bt: CVE-2019-12554
