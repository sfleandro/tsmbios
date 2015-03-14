# Description #

The information in this structure defines attributes of the overall system and is intended to be associated with the Component ID group of the system’s MIF. An SMBIOS implementation is associated with a single
system instance and contains one and only one System Information (Type 1) structure.


# Sample code #

```
{$APPTYPE CONSOLE}

uses
  Classes,
  SysUtils,
  uSMBIOS in '..\..\Common\uSMBIOS.pas';

procedure GetSystemInfo;
Var
  SMBios : TSMBios;
  LSystem: TSystemInformation;
  UUID   : Array[0..31] of AnsiChar;
begin
  SMBios:=TSMBios.Create;
  try
    LSystem:=SMBios.SysInfo;
    WriteLn('System Information');
    WriteLn('Manufacter    '+LSystem.ManufacturerStr);
    WriteLn('Product Name  '+LSystem.ProductNameStr);
    WriteLn('Version       '+LSystem.VersionStr);
    WriteLn('Serial Number '+LSystem.SerialNumberStr);
    BinToHex(@LSystem.RAWSystemInformation.UUID,UUID,SizeOf(LSystem.RAWSystemInformation.UUID));
    WriteLn('UUID          '+UUID);
    if SMBios.SmbiosVersion>='2.4' then
    begin
      WriteLn('SKU Number    '+LSystem.SKUNumberStr);
      WriteLn('Family        '+LSystem.FamilyStr);
    end;
    WriteLn;
  finally
   SMBios.Free;
  end;
end;

begin
 try
    GetSystemInfo;
 except
    on E:Exception do
        Writeln(E.Classname, ':', E.Message);
 end;
 Writeln('Press Enter to exit');
 Readln;
end.

```