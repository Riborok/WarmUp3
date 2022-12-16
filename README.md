# WarmUp №3. Disks
---
### Task:
![The task](https://i.imgur.com/87mPukw.png)

>The program calculates the lowest cost to buy n discs.

#### Language: Delphi

### Code:
``` pascal
Program WarmUp3;
{
 Enter the cost of the disk;
 the cost of the box and how many disks it contains;
 the cost of the chest and how many boxes it contains.
 Enter the amount of disks to buy at the best price.
}

{$APPTYPE CONSOLE}

Var
  chest, box, discs, DiscInBox, BoxInChest, DiscInChest, InitialAmount, TotalAmount : integer;
  CostOfChest, CostOfBox, CostOfDisc, PriceChest, PriceBox, TotalCost : real;
  //chest - amount of chests
  //box - amount of box
  //discs - amount of discs
  //DiscInBox - amount of discs in the box
  //BoxInChest - amount of boxes in the chest
  //DiscInChest - amount of discs in the chest
  //InitialAmount - initial amount of discs for purchase
  //TotalAmount - bought in the end at the lowest price
  //CostOfChest - cost of the chest
  //CostOfBox - cost of the box
  //CostOfDisc - cost of the disc
  //PriceChest - price per disc
  //PriceBox - price per disc
  //TotalCost - lowest price

Begin
  Writeln('Input condition! Either entered number must be >0.');
  Writeln('Also amount of discs in the box, amount of boxes in the chest and amount of discs must be an integer.');
  Writeln;
  Writeln('Enter the cost of one disc');
  Readln(CostOfDisc);
  Writeln('Enter the cost of one box and the amount of discs in the box');
  Read(CostOfBox);
  Readln(DiscInBox);
  Writeln('Enter the cost of one chest and the amount of boxes in the chest');
  Read(CostOfChest);
  Readln(BoxInChest);

  //Сonvert to the amount of disks in the chest
  DiscInChest:=BoxInChest * DiscInBox;

  Writeln('How many discs do you need to buy?');
  Readln(InitialAmount);

  //Validation of correct data
  if (CostOfDisc<=0) or (CostOfBox<=0) or (DiscInBox<=0) or (CostOfChest<=0) or (BoxInChest<=0) or (InitialAmount<=0) then
    Writeln('Input condition violated! Restart the program.')
  else
  begin

    //Assign how many disks need to buy
    discs:= InitialAmount;

    //Price per disc(for chest)
    PriceChest:=CostOfChest / DiscInChest;

    //Price per disc(for box)
    PriceBox:=CostOfBox / DiscInBox;

    //Checking if it is profitable to buy chests
    if (PriceChest <= PriceBox) and (PriceChest <= CostOfDisc) then
    begin

      //Buy the maximum amount of chests
      chest:= discs div DiscInChest;

      //Find the remainder after buying chests
      discs:= discs mod DiscInChest;

      //Checking if it is profitable to buy boxes
      if (PriceBox <= CostOfDisc) then
      begin

        //Buy the maximum amount of box
        box:= discs div DiscInBox;

        //Find the remainder after buying boxes
        discs:= discs mod DiscInBox;

        //Checking if it is profitable to take one more box
        if (discs*CostOfDisc) >= CostOfBox then
        begin
          discs:= 0;
          box:= box + 1;
        end;

      end;

      //Checking if it is profitable to take one more chest
      if (box*CostOfBox + discs*CostOfDisc) >= CostOfChest then
      begin
        box:= 0;
        discs:= 0;
        chest:= chest + 1;
      end;
    end

    //If it is unprofitable to buy chests, check the boxes
    else if (PriceBox <= CostOfDisc) then
    begin

      //Buy the maximum amount of box
      box:= discs div DiscInBox;

      //Find the remainder after buying boxes
      discs:= discs mod DiscInBox;

      //Checking if it is profitable to take one more box
      if (discs*CostOfDisc) >= CostOfBox then
      begin
        discs:= 0;
        box:= box + 1;
      end;
    end;

    TotalAmount:= chest*DiscInChest + box*DiscInBox + discs;
    TotalCost:= chest*CostOfChest + box*CostOfBox + discs*CostOfDisc;

    Write('The lowest price turned out - ', TotalCost:0:1, ' rub, bought at this price ',TotalAmount, ' discs. (');
    Write(TotalAmount-InitialAmount, ' discs more than planned). ');
    Writeln('Of which ',chest,' chests, ',box,' boxes, ',discs,' discs.');

    //Consider how many disks to buy, if take only them
    discs:= InitialAmount;

    //Consider how many box to buy, if take only them
    box:= Trunc(InitialAmount/DiscInBox);
    if Frac(InitialAmount/DiscInBox)>0 then
      box:= box + 1;

    //Consider how many box to chest, if take only them
    chest:= Trunc(InitialAmount/DiscInChest);
    if Frac(InitialAmount/DiscInChest)>0 then
      chest:= chest + 1;

    //Write the amount and price if take only a definite type
    Writeln('If take only disks, you get: ',discs,' discs, per price: ', (discs*CostOfDisc):0:1);
    Writeln('If take only boxes, you get: ',box,' boxes(',box*DiscInBox,' discs), per price: ', (box*CostOfBox):0:1);
    Writeln('If take only chests, you get: ',chest,' chests(',box*DiscInChest,' discs), per price: ', (chest*CostOfChest):0:1);

  end;

  Readln;
End.
```