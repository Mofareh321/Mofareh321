234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980
//@version=5indicator("SSL Channel", shorttitle="SSL Channel", overlay=true, timeframe="", timeframe_gaps=false)wicks = input(false, "Take Wicks into Account ?")highlightState = input(true, "Highlight State ?")ma(source, length, type) =>    type == "SMA" ? ta.sma(source, length) :     type == "EMA" ? ta.ema(source, length) :     type == "SMMA (RMA)" ? ta.rma(source, length) :     type == "WMA" ? ta.wma(source, length) :     type == "VWMA" ? ta.vwma(source, length) :     nashow_ma1   = input(true   , "MA High", inline="MA #1", group="Channel №1")ma1_type   = input.string("SMA"  , ""     , inline="MA #1", options=["SMA", "EMA", "SMMA (RMA)", "WMA", "VWMA"], group="Channel №1")ma1_source = input(high  , ""     , inline="MA #1", group="Channel №1")ma1_length = input.int(200     , ""     , inline="MA #1", minval=1, group="Channel №1")ma1_color  = input( color.green, ""     , inline="MA #1", group="Channel №1")ma1 = ma(ma1_source, ma1_length, ma1_type)show_ma2   = input(true   , "MA Low", inline="MA #2", group="Channel №1")ma2_type   = input.string("SMA"  , ""     , inline="MA #2", options=["SMA", "EMA", "SMMA (RMA)", "WMA", "VWMA"], group="Channel №1")ma2_source = input(low  , ""     , inline="MA #2", group="Channel №1")ma2_length = input.int(200     , ""     , inline="MA #2", minval=1, group="Channel №1")ma2_color  = input( color.red, ""     , inline="MA #2", group="Channel №1")ma2 = ma(ma2_source, ma2_length, ma2_type)showLabels1 = input(true, "Show Buy/Sell Labels ?", group="Channel №1")show_ma3   = input(false   , "MA High", inline="MA #3", group="Channel №2")ma3_type   = input.string("SMA"  , ""     , inline="MA #3", options=["SMA", "EMA", "SMMA (RMA)", "WMA", "VWMA"], group="Channel №2")ma3_source = input(high  , ""     , inline="MA #3", group="Channel №2")ma3_length = input.int(20    , ""     , inline="MA #3", minval=1, group="Channel №2")ma3_color  = input( color.orange, ""     , inline="MA #3", group="Channel №2")ma3 = ma(ma3_source, ma3_length, ma3_type)show_ma4   = input(false   , "MA Low", inline="MA #4", group="Channel №2")ma4_type   = input.string("SMA"  , ""     , inline="MA #4", options=["SMA", "EMA", "SMMA (RMA)", "WMA", "VWMA"], group="Channel №2")ma4_source = input(low  , ""     , inline="MA #4", group="Channel №2")ma4_length = input.int(20    , ""     , inline="MA #4", minval=1, group="Channel №2")ma4_color  = input( color.blue, ""     , inline="MA #4", group="Channel №2")ma4 = ma(ma4_source, ma4_length, ma4_type)showLabels2 = input(true, "Show Buy/Sell Labels ?", group="Channel №2")Hlv1 = float(na)Hlv1 := (wicks ? high : close) > ma1 ? 1 : (wicks ? low : close) < ma2 ? -1 : Hlv1[1]sslUp1   = Hlv1 < 0 ? ma2 : ma1sslDown1 = Hlv1 < 0 ? ma1 : ma2Color1 = Hlv1 == 1 ? ma1_color : ma2_colorfillColor1 = highlightState ? (color.new(Color1, 90)) : nahighLine1 = plot(show_ma1 ? sslUp1 : na, title="UP", linewidth=2, color = Color1)lowLine1 = plot(show_ma2 ? sslDown1 : na, title="DOWN", linewidth=2, color = Color1)plotshape(show_ma1 and showLabels1 and Hlv1 == 1 and Hlv1[1] == -1, title="Buy Label", text="Buy", location=location.belowbar, style=shape.labelup, size=size.tiny, color=Color1, textcolor= color.white)plotshape(show_ma2 and showLabels1 and Hlv1 == -1 and Hlv1[1] == 1, title="Sell Label", text="Sell", location=location.abovebar, style=shape.labeldown, size=size.tiny, color=Color1, textcolor= color.white)fill(highLine1, lowLine1, color = fillColor1)Hlv2 = float(na)Hlv2 := (wicks ? high : close) > ma3 ? 1 : (wicks ? low : close) < ma4 ? -1 : Hlv2[1]sslUp2   = Hlv2 < 0 ? ma4 : ma3sslDown2 = Hlv2 < 0 ? ma3 : ma4Color2 = Hlv2 == 1 ? ma3_color : ma4_colorfillColor2 = highlightState ? (color.new(Color2, 90)) : nahighLine2 = plot(show_ma3 ? sslUp2 : na, title="UP", linewidth=2, color = Color2)lowLine2 = plot(show_ma4 ? sslDown2 : na, title="DOWN", linewidth=2, color = Color2)plotshape(show_ma3 and showLabels2 and Hlv2 == 1 and Hlv2[1] == -1, title="Buy Label", text="Buy", location=location.belowbar, style=shape.labelup, size=size.tiny, color=Color2, textcolor= color.white)plotshape(show_ma4 and showLabels2 and Hlv2 == -1 and Hlv2[1] == 1, title="Sell Label", text="Sell", location=location.abovebar, style=shape.labeldown, size=size.tiny, color=Color2, textcolor= color.white)fill(highLine2, lowLine2, color = fillColor2)// Alertsalertcondition(Hlv1 == 1 and Hlv1[1] == -1, title="SSL Channel (1) Buy Alert", message = "SSL Channel (1): BUY")alertcondition(Hlv1 == -1 and Hlv1[1] == 1, title="SSL Channel (1) Sell Alert", message = "SSL Channel (1): SELL")alertcondition(Hlv2 == 1 and Hlv2[1] == -1, title="SSL Channel (2) Buy Alert", message = "SSL Channel (2): BUY")alertcondition(Hlv2 == -1 and Hlv2[1] == 1, title="SSL Channel (2) Sell Alert", message = "SSL Channel (2): SELL"
//@version=4
//Basic Hull Ma Pack tinkered by InSilico 
study("Hull Suite by InSilico", overlay=true)

//INPUT
src = input(close, title="Source")
modeSwitch = input("Hma", title="Hull Variation", options=["Hma", "Thma", "Ehma"])
length = input(55, title="Length(180-200 for floating S/R , 55 for swing entry)")
lengthMult = input(1.0, title="Length multiplier (Used to view higher timeframes with straight band)")

useHtf = input(false, title="Show Hull MA from X timeframe? (good for scalping)")
htf = input("240", title="Higher timeframe", type=input.resolution)

switchColor = input(true, "Color Hull according to trend?")
candleCol = input(false,title="Color candles based on Hull's Trend?")
visualSwitch  = input(true, title="Show as a Band?")
thicknesSwitch = input(1, title="Line Thickness")
transpSwitch = input(40, title="Band Transparency",step=5)

//FUNCTIONS
//HMA
HMA(_src, _length) =>  wma(2 * wma(_src, _length / 2) - wma(_src, _length), round(sqrt(_length)))
//EHMA    
EHMA(_src, _length) =>  ema(2 * ema(_src, _length / 2) - ema(_src, _length), round(sqrt(_length)))
//THMA    
THMA(_src, _length) =>  wma(wma(_src,_length / 3) * 3 - wma(_src, _length / 2) - wma(_src, _length), _length)
    
//SWITCH
Mode(modeSwitch, src, len) =>
      modeSwitch == "Hma"  ? HMA(src, len) :
      modeSwitch == "Ehma" ? EHMA(src, len) : 
      modeSwitch == "Thma" ? THMA(src, len/2) : na

//OUT
_hull = Mode(modeSwitch, src, int(length * lengthMult))
HULL = useHtf ? security(syminfo.ticker, htf, _hull) : _hull
MHULL = HULL[0]
SHULL = HULL[2]

//COLOR
hullColor = switchColor ? (HULL > HULL[2] ? #00ff00 : #ff0000) : #ff9800

//PLOT
///< Frame
Fi1 = plot(MHULL, title="MHULL", color=hullColor, linewidth=thicknesSwitch, transp=50)
Fi2 = plot(visualSwitch ? SHULL : na, title="SHULL", color=hullColor, linewidth=thicknesSwitch, transp=50)
alertcondition(crossover(MHULL, SHULL), title="Hull trending up.", message="Hull trending up.")
alertcondition(crossover(SHULL, MHULL), title="Hull trending down.", message="Hull trending down.")
///< Ending Filler
fill(Fi1, Fi2, title="Band Filler", color=hullColor, transp=transpSwitch)
///BARCOLOR
barcolor(color = candleCol ? (switchColor ? hullColor : na) : na)
