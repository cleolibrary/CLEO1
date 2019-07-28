# CLEO v1
The original SCM-based implementation of CLEO. [Sanny Builder](https://sannybuilder.com/index.html) is required to compile.

https://gtaforums.com/topic/268976-project-cleo/

```
{$VERSION 3.1.0020}

// Provides executing each time the game started
gosub @CLEO_RUN

03A4: name_thread 'MAIN'
var              
 $PLAYER_CHAR: Player 
end // var
01F0: set_max_wanted_level_to 6 
0111: toggle_wasted_busted_check 0 
00C0: set_current_time 15 0 
04E4: unknown_refresh_game_renderer_at 824.0641 -1794.8955 
Camera.SetAtPos(2488.562, -1666.865, 12.8757)
$PLAYER_CHAR = Player.Create(#NULL, 2488.562, -1666.865, 12.8757)
$PLAYER_ACTOR = Actor.EmulateFromPlayer($PLAYER_CHAR)
07AF: $PLAYER_GROUP = player $PLAYER_CHAR group
Camera.SetBehindPlayer
set_weather 0          
wait 0 ms                                
$PLAYER_CHAR.SetClothes("PLAYER_FACE", "HEAD", Head)
$PLAYER_CHAR.SetClothes("JEANSDENIM", "JEANS", Legs)
$PLAYER_CHAR.SetClothes("SNEAKERBINCBLK", "SNEAKER", Shoes)
$PLAYER_CHAR.SetClothes("VEST", "VEST", Torso)
$PLAYER_CHAR.Build
$PLAYER_CHAR.CanMove = True
fade 1 (out) 0 ms                     
select_interior 0
0629: change_stat 181 (islands unlocked) to 4
016C: restart_if_wasted at 2027.77 -1420.52 15.99 angle 137.0 for_town_number 0
016D: restart_if_busted at 1550.68 -1675.49 14.51 angle 90.0 for_town_number 0
0180: set_on_mission_flag_to $ONMISSION // Note: your missions have to use the variable defined here ($ONMISSION)
03E6: remove_text_box
0330: toggle_player $PLAYER_CHAR infinite_run 1

// show custom text
0A8E: 0@ = 0xA49964 + @_TextPtr // MainScm + 2 bytes of opcode + 2 bytes of datatype and string length + label
0AA5: call 0x588BE0 num_params 4 pop 4 0 0 0 0@

// create a car with the only opcode
0AA5: call 0x43A0B6 num_params 1 pop 1 #INFERNUS

end_thread

:_TextPtr
0900: "Custom Text Here"
0000: null-terminator

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

     Project "CLEO"

      v1.29 Final
 
   by Seemann (c) 2007

 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
 http://www.gtaforums.com/index.php?showtopic=268976
 http://www.sannybuilder.com/forums/viewtopic.php?id=62
       
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~    
      Opcodes List:
 
  0A8C: write_memory <dword> size <byte> value <dword> virtual_protect <bool>
  0A8D: <var> = read_memory <int32> size <byte> virtual_protect <bool>
  0A8E: <var> = <valA> + <valB> // int
  0A8F: <var> = <valA> - <valB> // int
  0A90: <var> = <valA> * <valB> // int
  0A91: <var> = <valA> / <valB> // int
  
  0A96: <var> = actor <handle> struct
  0A97: <var> = car <handle> struct    
  0A98: <var> = object <handle> struct

  0A99: chdir <flag>
  0A9A: <var> = openfile "path" mode <dword>
  0A9B: closefile <hFile>   
  0A9C: <var> = file <hFile> size
  0A9D: readfile <hFile> size <dword> to <var>  
  0A9E: writefile <hFile> size <dword> from <var>

  0A9F: <var> = current_thread_address
  0AA0: gosub_if_false <label>
  0AA1: return_if_false

  0AA2: <var> = load_library "path"
  0AA3: free_library <hLib>
  0AA4: <var> = get_proc_address "name" library <hLib>

  0AA5: call <address> num_params <byte> pop <byte> [param1, param2...]
  0AA6: call_method <address> struct <address> num_params <byte> pop <byte> [param1, param2...]
          
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      SASCM.INI Lines
 
  0A8C=4,write_memory %1d% size %2d% value %3d% virtual_protect %4d%
  0A8D=4,%4d% = read_memory %1d% size %2d% virtual_protect %3d%
  0A8E=3,%3d% = %1d% + %2d% ; int
  0A8F=3,%3d% = %1d% - %2d% ; int
  0A90=3,%3d% = %1d% * %2d% ; int
  0A91=3,%3d% = %1d% / %2d% ; int

  0A96=2,%2d% = actor %1d% struct
  0A97=2,%2d% = car %1d% struct
  0A98=2,%2d% = object %1d% struct

  0A99=1,chdir %1b:userdir/rootdir%
  0A9A=3,%3d% = openfile %1s% mode %2d% // IF and SET
  0A9B=1,closefile %1d%
  0A9C=2,%2d% = file %1d% size
  0A9D=3,readfile %1d% size %2d% to %3d%
  0A9E=3,writefile %1d% size %2d% from %3d%
  0A9F=1,%1d% = current_thread_pointer

  0AA0=1,gosub_if_false %1p%
  0AA1=0,return_if_false
  
  0AA2=2,%2h% = load_library %1s% // IF and SET
  0AA3=1,free_library %1h%
  0AA4=3,%3d% = get_proc_address %1s% library %2d%
  0AA5=-1,call %1d% num_params %2h% pop %3h%
  0AA6=-1,call_method %1d% struct %2d% params %3h% pop %4h%  
                       
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
         
:CLEO_RUN
0@ = -429566
&0(0@,1i) == 4611680
jf @CLEO_v2 // 1.0
0@ = -429539
&0(0@,1i) =  0xA49960
&0(0@,1i) += @CLEO_HANDLER
0485: return_true
return

:CLEO_v2
059A: return_false
return

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 DO NOT EVER CHANGE THE HANDLER CODE!

 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
}
:CLEO_HANDLER //  0A8C - 0AEF
hex
 {
  EAX = CurrentOpcode
  ECX = ThreadPointer
  EDX = EIP
  ESI = ECX
  ESP +0 = ret_addr
      +4 = opcode
      +8 = ecx
        
  DO NOT CHANGE ESI!
 } 
 8B 44 24 04            // mov eax, [esp+4+Opcode]
 66 2D 8C 0A            // sub ax, 0x0A8C
 BA @CLEO_Pointers      // mov edx, @CLEO_Pointers
 8B 94 82 60 99 A4 00   // mov edx, [0xA49960+eax*4+edx]
 81 C2 60 99 A4 00      // add edx, 0xA49960
 FF E2                  // jmp edx
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    CLEO Pointers Table
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Pointers
hex
 @CLEO_Opcode0A8C
 @CLEO_Opcode0A8D
 @CLEO_Opcode0A8E
 @CLEO_Opcode0A8F
 @CLEO_Opcode0A90
 @CLEO_Opcode0A91
 @CLEO_Opcode0A92 // reserved
 @CLEO_Opcode0A93 // reserved
 @CLEO_Opcode0A94 // reserved  
 @CLEO_Opcode0A95 // reserved
 @CLEO_Opcode0A96     
 @CLEO_Opcode0A97
 @CLEO_Opcode0A98  
 @CLEO_Opcode0A99 
 @CLEO_Opcode0A9A
 @CLEO_Opcode0A9B
 @CLEO_Opcode0A9C 
 @CLEO_Opcode0A9D 
 @CLEO_Opcode0A9E
 @CLEO_Opcode0A9F
 @CLEO_Opcode0AA0
 @CLEO_Opcode0AA1
 @CLEO_Opcode0AA2
 @CLEO_Opcode0AA3   
 @CLEO_Opcode0AA4 
 @CLEO_Opcode0AA5 
 @CLEO_Opcode0AA6 
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A8C
 0A8C: write_memory (1) size (2) value (3) virtual_protect (4)
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}

:CLEO_Opcode0A8C
hex
 6A 04                  // push 4
 B8 80 40 46 00 FF D0   // call CollectNumberParams

 83 3D 84 3C A4 00 01   // cmp VirtualProtect, 1  (p4)
 75 08                  // jnz @MOVedx
 6A 04                  // push 4
 E8 40 00 00 00         // call VirtualProtect+64
 58                     // pop eax

 8B 15 78 3C A4 00      // mov edx, Address (p1)
 8B 0D 7C 3C A4 00      // mov ecx, Size    (p2)
 8B 05 80 3C A4 00      // mov eax, Value   (p3)

 83 F9 01               // cmp Size, 1 
 75 04                  // jnz @word
 88 02                  // mov [Address], al
 EB 0C                  // jmp @ret  
 83 F9 02               // cmp Size, 2
 75 05                  // jnz @dword
 66 89 02               // mov [Address], ax
 EB 02                  // jmp @ret
 89 02                  // mov [Address], eax

 83 3D 84 3C A4 00 01   // cmp VirtualProtect, 1  (p4)
 75 0C                  // jnz @ret
 FF 35 F4 3C A4 00      // push oldProtect
 E8 04 00 00 00         // call VirtualProtect+4
 58                     // pop eax
 32 C0                  // xor al, al 
 C2 04 00               // retn 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 CLEO Virtual Protect Subroutine
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
hex
 68 F4 3C A4 00         // push offset oldProtect
 FF 74 24 08            // push [esp+8+NewProtect]
 FF 35 7C 3C A4 00      // push Size
 FF 35 78 3C A4 00      // push Address
 FF 15 2C 80 85 00      // call VirtualProtect
 C3                     // ret
end
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A8D
 0A8D: (1) = read_memory (2) size (3) virtual_protect (4)
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A8D
hex
 6A 03                  // push 3
 B8 80 40 46 00 FF D0   // call CollectNumberParams

 83 3D 80 3C A4 00 01   // cmp VirtualProtect, 1  (p3)
 75 08                  // jnz @MOVedx
 6A 04                  // push 4
 E8 CB FF FF FF         // call VirtualProtect-53
 58                     // pop eax

 8B 05 78 3C A4 00      // mov eax, Address (p1)
 31 C9                  // xor ecx, ecx
 BA 78 3C A4 00         // mov edx, offset Result (p1)
 89 0A                  // mov [edx], ecx
 8B 0D 7C 3C A4 00      // mov ecx, Size (p2)

 83 F9 01               // cmp ecx, 1
 75 06                  // jnz @2
 8A 00                  // mov al, [eax]
 88 02                  // mov [edx], al
 EB 11                  // jmp @write 

 83 F9 02               // cmp ecx, 2
 75 08                  // jnz @4
 66 8B 00               // mov ax, [eax]
 66 89 02               // mov [edx], ax
 EB 04                  // jmp @write

 8B 00                  // mov eax, [eax]
 89 02                  // mov [edx], eax

 83 3D 80 3C A4 00 01   // cmp VirtualProtect, 1  (p3)
 75 0C                  // jnz @ret
 FF 35 F4 3C A4 00      // push oldProtect
 E8 85 FF FF FF         // call VirtualProtect-123
 58                     // pop eax

 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 B8 70 43 46 00 FF D0   // call WriteResult
 32 C0                  // xor al, al  
 C2 04 00               // retn 4
end


{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A8E
  0A8E: (1) = (2) + (3) // int
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A8E
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A8F
  0A8F: (1) = (2) - (3) // int
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A8F
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A90
  0A90: (1) = (2) * (3) // int
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A90
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A91
  0A91: (1) = (2) / (3) // int
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A91
hex
 50                     // push eax
 6A 02                  // push 2
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 A1 78 3C A4 00         // mov eax, p1
 8B 15 7C 3C A4 00      // mov edx, p2 
 59                     // pop ecx
 // ADD EAX, EDX
 83 F9 02               // cmp ecx, 2 (opcode 0A8E)
 75 04                  // jnz @opcode0A8F 
 01 D0                  // add eax, edx
 EB 19                  // jmp @write
 // SUB EAX, EDX
 83 F9 03               // cmp ecx, 3 (opcode 0A8F)
 75 04                  // jnz @opcode0A90 
 29 D0                  // sub eax, edx
 EB 10                  // jmp @write
 // MUL EAX, EDX
 83 F9 04               // cmp ecx, 4 (opcode 0A90)
 75 04                  // jnz @opcode0A91 
 F7 EA                  // imul edx
 EB 07                  // jmp @write
 // DIV EAX, P2
 99                     // cdq
 F7 3D 7C 3C A4 00      // idiv p2    (opcode 0A91)
 A3 78 3C A4 00         // mov p1, eax
 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 B8 70 43 46 00 FF D0   // call WriteResult
 32 C0                  // xor al, al
 C2 04 00               // retn 4
end 

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A92
  0A92: (1) = (2) + (3) // float
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A92
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A93
  0A93: (1) = (2) - (3) // float
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A93
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A94
  0A94: (1) = (2) * (3) // float
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A94
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A95
  0A95: (1) = (2) / (3) // float
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A95
hex
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A96
  0A96: (1) = actor (2) struct
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A96
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A97
  0A97: (1) = car (2) struct
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A97
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A98
  0A98: (1) = object (2) struct
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A98
hex
 50                     // push eax
 6A 01                  // push 1
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 59                     // pop ecx

 FF 35 78 3C A4 00      // push Handle (p1)
 // 0A96: ACTOR.STRUCT
 83 F9 0A               // cmp ecx, 10 (opcode 0A96)
 75 0F                  // jnz @opcode0A97 
 8B 0D 90 44 B7 00      // mov ecx, @CActors
 B8 10 49 40 00 FF D0   // call GetActorPointer
 EB 21                  // jmp @write 
 // 0A97: CAR.STRUCT
 83 F9 0B               // cmp ecx, 11 (opcode 0A97)
 75 0F                  // jnz @opcode0A98 
 8B 0D 94 44 B7 00      // mov ecx, @CVehicles
 B8 E0 48 40 00 FF D0   // call GetCarPointer
 EB 0D                  // jmp @write 
 // 0A98: OBJECT.STRUCT
 8B 0D 9C 44 B7 00      // mov ecx, @CObjects (opcode 0A98)
 B8 40 50 46 00 FF D0   // call GetObjectPointer

 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 A3 78 3C A4 00         // mov p1, eax
 B8 70 43 46 00 FF D0   // call WriteResult
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A99
  0A99: chdir <flag>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A99
hex
 6A 01                  // push 1
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 // test flag
 A1 78 3C A4 00         // mov eax, Flag (p1)
 85 C0                  // test eax, eax
 74 0E                  // jz @root
 83 F8 01               // cmp eax, 1
 74 15                  // jnz @exit
 // Flag=1; UserDir
 B8 60 88 53 00 FF D0   // call SetUserDirToCurrent
 EB 0F                  // jmp @exit                       
 // Flag=0; RootDir
 68 54 8B 85 00         // push offset Null
 B8 D0 87 53 00 FF D0   // call ChDir 
 83 C4 04               // add esp, 4
 32 C0                  // xor al, al
 C2 04 00               // ret 4    
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A9A
  0A9A: <var> = openfile "path" mode <dword>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A9A
hex       
 81 EC 80 00 00 00      // sub esp, 128
 6A 64                  // push 100
 8D 44 24 04            // lea eax, [esp+4]
 50                     // push eax

 B8 50 3D 46 00 FF D0   // call GetStringParam
 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 B8 80 40 46 00 FF D0   // call CollectNumberParams

 68 78 3C A4 00         // push offset p1
 8D 44 24 04            // lea eax, [esp+4]
 50                     // push eax

 B8 00 89 53 00 FF D0   // call fopen

 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 A3 78 3C A4 00         // mov p1, eax
 
 31 D2                  // xor edx, edx
 85 C0                  // test eax, eax
 0F 95 C2               // setnz dl
 52                     // push edx
 B8 D0 59 48 00 FF D0   // call SetConditionResult

 B8 70 43 46 00 FF D0   // call WriteResult
 81 C4 88 00 00 00      // add esp, 136
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A9B
  0A9B: closefile <hFile>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A9B
hex
 6A 01                  // push 1
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 FF 35 78 3C A4 00      // push hFile (p1)
 B8 D0 89 53 00 FF D0   // call CloseFile
 58                     // pop eax
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A9C
  0A9C: <var> = file <hFile> size 
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A9C
hex
 6A 01                  // push 1
 B8 80 40 46 00 FF D0   // call CollectNumberParams

 FF 35 78 3C A4 00      // push hFile (p1)
 B8 E0 89 53 00 FF D0   // call GetFileSize
 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 A3 78 3C A4 00         // mov p1, eax
 B8 70 43 46 00 FF D0   // call WriteResult

 58                     // pop eax
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end


{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A9D
  0A9D: readfile <hFile> size <dword> to <var>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A9D
{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A9E
  0A9E: writefile <hFile> size <dword> from <var>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A9E
hex
 50                     // push eax                                   
 6A 02                  // push 2
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 6A 02                  // push 2
 B8 90 47 46 00 FF D0   // call GetVariablePos
 59                     // pop ecx 

 FF 35 7C 3C A4 00      // push Size (p2)
 50                     // push eax
 FF 35 78 3C A4 00      // push hFile (p1)

 83 F9 11               // cmp ecx, 17 (opcode 0A9D)
 75 07                  // jnz @opcode0A9E 
 B8 50 89 53 00         // mov eax, BlockRead
 EB 05                  // jmp @call
 B8 70 89 53 00         // mov eax, BlockWrite

 FF D0                  // call eax
 83 C4 0C               // add esp, 12
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0A9F
  0A9F: <var> = current_thread_address
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0A9F
hex
 6A 01                  // push 1
 89 35 78 3C A4 00      // mov p1, esi
 B8 70 43 46 00 FF D0   // call WriteResult
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0AA0
  0AA0: gosub_if_false <label>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0AA0
hex
 6A 01                  // push 1
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 
 8A 86 C5 00 00 00      // mov al, [esi+0xC5+IfResult]
 84 C0                  // test al, al
 75 1C                  // jnz @true
  
 0F B6 46 38            // movzx eax, [esi+0x38+THREAD.StackCounter]
 8B 56 14               // mov edx, [esi+0x14+THREAD.CurrentIP]
 89 54 86 18            // mov [esi+eax*4+THREAD.ReturnStack], edx
 66 FF 46 38            // inc word ptr [esi+0x38+THREAD.StackCounter]

 FF 35 78 3C A4 00      // push label (p1)                                   
 B8 A0 4D 46 00 FF D0   // call SetJumpLocation
 
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0AA1
  0AA1: return_if_false
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0AA1
hex
 8A 86 C5 00 00 00      // mov al, [esi+0xC5+IfResult]
 84 C0                  // test al, al
 75 0F                  // jnz @true

 66 FF 4E 38            // dec word ptr [esi+0x38+THREAD.StackCounter]
 0F B6 46 38            // movzx eax, [esi+0x38+THREAD.StackCounter] 
 8B 54 86 18            // mov edx, [esi+eax*4+0x18+THREAD.ReturnStack]
 89 56 14               // mov [esi+0x14+CurrentIP], edx
 
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0AA2
  0AA2: <var> = load_library <path>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0AA2
hex
 81 EC 80 00 00 00      // sub esp, 128
 6A 64                  // push 100
 8D 44 24 04            // lea eax, [esp+4]
 50                     // push eax

 B8 50 3D 46 00 FF D0   // call GetStringParam
 8D 04 24               // lea eax, esp
 50                     // push eax
 FF 15 70 80 85 00      // call LoadLibraryA

 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 A3 78 3C A4 00         // mov p1, eax
 
 31 D2                  // xor edx, edx
 85 C0                  // test eax, eax
 0F 95 C2               // setnz dl
 52                     // push edx
 B8 D0 59 48 00 FF D0   // call SetConditionResult

 B8 70 43 46 00 FF D0   // call WriteResult
 81 C4 80 00 00 00      // add esp, 128
 
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0AA3
  0AA3: free_library <hLib>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0AA3
hex
 6A 01                  // push 1
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 FF 35 78 3C A4 00      // push hLib (p1)
 FF 15 10 81 85 00      // call FreeLibrary
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0AA4
  0AA4: <var> = get_proc_address "name" library <hLib>
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0AA4
hex
 81 EC 80 00 00 00      // sub esp, 128
 6A 64                  // push 100
 8D 44 24 04            // lea eax, [esp+4]
 50                     // push eax

 B8 50 3D 46 00 FF D0   // call GetStringParam
 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 B8 80 40 46 00 FF D0   // call CollectNumberParams

 8D 04 24               // lea eax, esp
 50                     // push eax
 FF 35 78 3C A4 00      // push hLib (p1)
 FF 15 6C 80 85 00      // call GetProcAddress
 
 6A 01                  // push 1
 8B CE                  // mov ecx, esi
 A3 78 3C A4 00         // mov p1, eax
 
 31 D2                  // xor edx, edx
 85 C0                  // test eax, eax
 0F 95 C2               // setnz dl
 52                     // push edx
 B8 D0 59 48 00 FF D0   // call SetConditionResult

 B8 70 43 46 00 FF D0   // call WriteResult
 81 C4 80 00 00 00      // add esp, 128
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0AA5
  0AA5: call <address> num_params <byte> pop <byte> [param1, param2...]
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0AA5
hex
 6A 03                  // push 3
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 53                     // push ebx
 57                     // push edi
 8B 1D 78 3C A4 00      // mov ebx, addr (p1)
 8B 3D 7C 3C A4 00      // mov edi, Number (p2)

 85 FF                  // test edi, edi
 74 12                  // jz @call
 6A 01                  // push 1
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 FF 35 78 3C A4 00      // push param (p1) 
 4F                     // dec edi
 EB EA                  // jmp @loop

 FF D3                  // call ebx
 
 A1 80 3C A4 00         // mov eax, pop
 6B C0 04               // imul eax, 4
 01 C4                  // add esp, eax
 
 5F                     // pop edi
 5B                     // pop ebx
 FF 46 14               // inc dword ptr [esi+0x14+CurrentIP]
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     Opcode 0AA6
  0AA6: call_method <address> struct <address> num_params <byte> pop <byte> [param1, param2...]
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
:CLEO_Opcode0AA6
hex
 6A 04                  // push 4
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 53                     // push ebx
 57                     // push edi
 51                     // push ecx
 8B 1D 78 3C A4 00      // mov ebx, addr (p1)
 8B 3D 80 3C A4 00      // mov edi, Number (p3)

 85 FF                  // test edi, edi
 74 12                  // jz @call
 6A 01                  // push 1
 B8 80 40 46 00 FF D0   // call CollectNumberParams
 FF 35 78 3C A4 00      // push param (p1) 
 4F                     // dec edi
 EB EA                  // jmp @loop

 8B 0D 7C 3C A4 00      // mov ecx, Struct (p2)
 FF D3                  // call ebx
 
 A1 84 3C A4 00         // mov eax, pop (p4)
 6B C0 04               // imul eax, 4
 01 C4                  // add esp, eax

 59                     // pop ecx 
 5F                     // pop edi
 5B                     // pop ebx
 FF 46 14               // inc dword ptr [esi+0x14+CurrentIP]
 32 C0                  // xor al, al
 C2 04 00               // ret 4
end

{
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     END OF CLEO
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
}
```
