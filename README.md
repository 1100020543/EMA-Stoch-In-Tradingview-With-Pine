# EMA-Stoch-In-Tradingview-With-Pine
Here is my tradingview pine script Trader Bot, You Can Easily Copy And Paste In Your Pine Editor
From Lovely BitBell




//////////////////////////////////////////////////////////////////////////////
Just Copy And Paste To Your Pine Script
/////////////////////////////////////////////////////////////////////////////////////
// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © CryptoButgi_Bot
//@version=5


strategy("EMA_STOCH_CryptoButgi_Bot_F14_MartinGale_With_Target_Rande", shorttitle="EMA_STOCH_CryptoButgi_Bot_F14_MartinGale_With_Target_Rande", overlay=true)
Ema50_Input = input(50,title = "EMA FAST")
Ema150_Input = input(150,title = 'EMA SLOW')
Ema50 = (ta.ema(close,Ema50_Input))
plot(Ema50, title="Ema50", color=color.rgb(62, 235, 99))
Ema150 = (ta.ema(close,Ema150_Input))
plot(Ema150, title="Ema150", color=#f60404)
Ema300 = ta.ema(close,300)
plot(Ema300, title="Ema300", color=#2f618d)
Ema50Upper = Ema50 * 1.005
Ema50Downer = Ema50 * 0.995
Ema150Upper = Ema150 * 1.005
Ema150Downer = Ema150 * 0.995

periodK = input(5, title="PK")
smoothK = input(5, title="SK")
periodD = input(5, title="D")
k = ta.sma(ta.stoch(close, high, low, periodK), smoothK)
d = ta.sma(k, periodD)

Candel_Week_Low_pwc_Last = request.security(syminfo.tickerid, 'W', low[1])
Candel_week_Low_pwc = request.security(syminfo.tickerid, 'W', low[1])
Candel_Week_Low_pwc_Last_Short = request.security(syminfo.tickerid, 'W', close[1])
Candel_week_Low_pwc_Short = request.security(syminfo.tickerid, 'W', close[1])



// var startUnitSize = 0.0
var unit_size = 0.0
var unit_Price = 0.0
var Martin_Count = 0.0
var Rande_Long = 0.0
var Rande_Long_Size = 0.0
var Rande_Price = 0.0

var unit_size_Short = 0.0
var unit_Price_Short = 0.0
var Martin_Count_Short = 0
var Rande_Short = 0.0
var Rande_Short_Size = 0.0
var Rande_Short_Price = 0.0


////////////////////Long
//////////////////////////////////////////////////////////////////////////
PositionEntry = strategy.opentrades.entry_price(0)
PositionTarget = PositionEntry * 1.01
PositionRandeTarget = Rande_Price * 1.007
PositionRande_Short_Target = Rande_Price * 0.993
PositionRande_Downer_Target = PositionEntry * 0.995
PositionRande_Upper_Target = PositionEntry * 1.006
PositionTargetTwo = PositionEntry * 1.02
////////////////////Short
PositionTargetShort = PositionEntry * 0.99
PositionTargetTwoShort = PositionEntry * 0.98
/////////////////////////////////////////////////////////////////////////


ShortRiskPercent = input.float(5.0,title = "Short_Risk_Percent")
LongRiskPercent = input.float(5.0,title = "Long_Risk_Percent")
ShortLeverage = input.float(20.0,title = "Short_Leverage")
LongLeverage = input.float(20.0,title = "Long_Leverage")
// Risk_Percentage = math.abs((Ema150Downer-close)*100/close)
Short_Risk_Percentage = math.abs((Ema150Upper-close)*100/close)

donchian(len) => math.avg(ta.lowest(len), ta.highest(len))
// baseLineStop = donchian(400)
// plot(baseLineStop, color=#ff0101, title="Base Line",linewidth = 4)
Candel_Day_Low_pwc = request.security(syminfo.tickerid, 'D', low[1])
Candel_Day_High_pwc = request.security(syminfo.tickerid, 'D', high[1])
StrategyEntryDownerz__MARTINgale = strategy.opentrades.entry_price(0) * 0.99
StrategyEntryDownerz__Shestoper = strategy.opentrades.entry_price(0) * 0.985

StrategyEntryDownerz__MARTINgale_Short = strategy.opentrades.entry_price(0) * 1.01
StrategyEntryDownerz__ShestoperShort = strategy.opentrades.entry_price(0) * 1.015
LongTwo = (strategy.opentrades.size(0) != 0) and (close < StrategyEntryDownerz__MARTINgale) and (strategy.opentrades.size(0) >= +0.0001)
ShortTwo = (strategy.opentrades.size(0) != 0) and (close > StrategyEntryDownerz__MARTINgale_Short) and (strategy.opentrades.size(0) <= -0.0001)
RSAa_Length = input(14,title = 'RSI')
RSAa = ta.rsi(close,RSAa_Length)
Risk_Percentage = math.abs((Ema150Downer-close)*100/close)

LongCondition = RSAa[3] <= RSAa[0] and RSAa[1] < 57.0 and (Ema150[2] > Ema300[2]) and (Ema50[1] > Ema150[1]) and ((close < Ema50Upper and close > Ema50Downer) or (close < Ema150Upper and close > Ema150Downer)) and k[1] <= 22.0 and k > 20 and (strategy.opentrades.size(0) <= 0)
LongConditionTwo = RSAa[1] < 45.0 and (Ema50[1] > Ema150[1]) and (close < Ema150Upper and close > Ema150Downer) and k[1] <= 22.0 and k > 20
if LongCondition
    Quisk = (LongRiskPercent / 100) * (strategy.initial_capital / close / Risk_Percentage * LongLeverage)
    strategy.entry("Long",strategy.long,qty = Quisk)
    unit_size := Quisk
    Martin_Count := 5
    Rande_Long := 1

if Rande_Long == 1 and LongConditionTwo and (close < PositionRande_Downer_Target or close > PositionRande_Upper_Target) and strategy.opentrades.size(0) > 0
    QuiskTwoThree = strategy.opentrades.size(0) / 2
    QuiskTwoThreez = QuiskTwoThree * +1
    strategy.entry("LongTwo",strategy.long,qty = QuiskTwoThreez)
    Rande_Long := 2
    Rande_Long_Size := QuiskTwoThreez
    Rande_Price := close

if LongTwo and Martin_Count == 5
    QuiskTwo = strategy.opentrades.size(0) * 2
    strategy.entry("Long",strategy.long,qty = QuiskTwo)
    Martin_Count := 2
    unit_size := unit_size + QuiskTwo

if (close >= PositionRandeTarget and Rande_Long == 2) and strategy.opentrades.size(0) > 0
    strategy.close("LongTwo",qty = Rande_Long_Size)
    Rande_Long := 1


if Ema50[2] >= Ema150[2] and Ema50[1] < Ema150[1]
    strategy.close_all()
if high <= Ema300 and high < Candel_Day_Low_pwc and high < StrategyEntryDownerz__Shestoper
    strategy.close_all()


if (close >= PositionTarget and Martin_Count == 2)
    strategy.close("Long",qty = unit_size * 0.3)
    Martin_Count := 3

if (close >= PositionTargetTwo and Martin_Count == 3)
    strategy.close("Long",qty = unit_size * 0.5)
    Martin_Count := 4




ShortCondition = RSAa[3] >= RSAa[0] and RSAa[1] > 35.0 and (Ema150[2] < Ema300[2]) and (Ema50[1] < Ema150[1]) and ((close < Ema50Upper and close > Ema50Downer) or (close < Ema150Upper and close > Ema150Downer)) and k[1] >= 75.0 and k < 80.0 and (strategy.opentrades.size(0) >= 0)
ShortConditionTwo = RSAa[1] > 67.0 and (Ema50[1] < Ema150[1]) and ((close < Ema150Upper and close > Ema150Downer)) and k[1] >= 75.0 and k < 80.0
if ShortCondition
    SQuisk = (ShortRiskPercent / 100) * (strategy.initial_capital / close / Short_Risk_Percentage * ShortLeverage)
    strategy.entry("Short",strategy.short,qty = SQuisk)
    unit_size_Short := SQuisk
    Martin_Count_Short := 5
    Rande_Short := 1


if ShortTwo and Martin_Count_Short == 5
    S2Quisk = strategy.opentrades.size(0) * -2
    strategy.entry("Short",strategy.short,qty = S2Quisk)
    Martin_Count_Short := 2
    unit_size_Short := unit_size_Short + S2Quisk

if Ema50[2] <= Ema150[2] and Ema50[1] > Ema150[1] and strategy.opentrades.size(0) < 0
    strategy.close("Short")
if low >= Ema300 and low > Candel_Day_High_pwc and low > StrategyEntryDownerz__ShestoperShort and strategy.opentrades.size(0) < 0
    strategy.close("Short")

if (close <= PositionTargetShort and Martin_Count_Short == 2) and strategy.opentrades.size(0) < 0
    strategy.close("Short",qty = unit_size_Short * 0.3)
    Martin_Count_Short := 3

if (close <= PositionTargetTwoShort and Martin_Count_Short == 3) and strategy.opentrades.size(0) < 0
    strategy.close("Short",qty = unit_size_Short * 0.5)
    Martin_Count_Short := 4



// if Rande_Short == 1 and ShortConditionTwo and (close < PositionRande_Downer_Target or close > PositionRande_Upper_Target) and strategy.opentrades.size(0) < 0
//     QuiskTwoThree = unit_size_Short / 2
//     // Rande_Short_Sizez = QuiskTwoThree * -1
//     strategy.entry("Short",strategy.short,qty = QuiskTwoThree)
//     // Rande_Short := 2
//     // Rande_Short_Size := Rande_Short_Sizez
//     // Rande_Short_Price := close
// if (close <= PositionRande_Short_Target and Rande_Short == 2) and strategy.opentrades.size(0) < 0
//     // Rande_Short_Sizez = Rande_Short_Size * 1
//     QuiskTwoThree = unit_size_Short / 2
//     strategy.close("Short",qty = QuiskTwoThree)
//     // Rande_Short := 1





// Short_PositionEntry = strategy.opentrades.entry_price(0)
// Short_PositionTarget = Short_PositionEntry * 0.994
// Short_PositionStop = Short_PositionEntry * 1.002
// Summy = (price[0] + (strategy.opentrades.entry_price(0))) / 2
// MarginSec = (strategy.opentrades.size(0) * 2)
// ,qty = (5.0 / 100) * (strategy.initial_capital / close / Risk_Percentage * 10)

// Risk_Percentage = math.abs((Weekelos-close)*100/close)
// plot(k, title="%K", color=#2962FF)blue
// plot(d, title="%D", color=#FF6D00)orarnge
// h0 = hline(80, "Upper Band", color=#787B86)
// hline(50, "Middle Band", color=color.new(#787B86, 50))
// h1 = hline(20, "Lower Band", color=#787B86)
// fill(h0, h1, color=color.rgb(33, 150, 243, 90), title="Background")
// 










