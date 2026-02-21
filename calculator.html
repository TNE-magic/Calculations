import { useState, useEffect, useRef, useCallback } from "react";

const STORAGE_KEY = "calc_memory_log";
const ACTIVE_KEY = "calc_active_memory";

export default function Calculator() {
  const [display, setDisplay] = useState("0");
  const [prevValue, setPrevValue] = useState(null);
  const [operator, setOperator] = useState(null);
  const [waitingForOperand, setWaitingForOperand] = useState(false);
  const [memoryLog, setMemoryLog] = useState([]);
  const [activeMemory, setActiveMemory] = useState(null);
  const [showMemoryOverlay, setShowMemoryOverlay] = useState(false);
  const [recalling, setRecalling] = useState(false);
  const [recallValue, setRecallValue] = useState(null);

  const digitTimerRef = useRef(null);
  const holdTimerRef = useRef({});
  const displayRef = useRef(display);
  displayRef.current = display;

  const prevValueRef = useRef(prevValue);
  prevValueRef.current = prevValue;
  const waitingRef = useRef(waitingForOperand);
  waitingRef.current = waitingForOperand;

  const isFirstNumber = prevValue === null && !waitingForOperand;

  useEffect(() => {
    const load = async () => {
      try { const r = await window.storage.get(STORAGE_KEY); if (r) setMemoryLog(JSON.parse(r.value)); } catch(e){}
      try { const r = await window.storage.get(ACTIVE_KEY); if (r) setActiveMemory(JSON.parse(r.value)); } catch(e){}
    };
    load();
  }, []);

  const saveToLog = useCallback(async (value, log) => {
    const entry = { value, timestamp: Date.now(), id: Math.random().toString(36).slice(2) };
    const newLog = [entry, ...log].slice(0, 50);
    setMemoryLog(newLog);
    setActiveMemory(value);
    try {
      await window.storage.set(STORAGE_KEY, JSON.stringify(newLog));
      await window.storage.set(ACTIVE_KEY, JSON.stringify(value));
    } catch(e){}
  }, []);

  useEffect(() => {
    if (digitTimerRef.current) clearTimeout(digitTimerRef.current);
    const digits = display.replace(/[^0-9]/g, "");
    if ((digits.length === 4 || digits.length === 6) && display !== "0" && isFirstNumber) {
      digitTimerRef.current = setTimeout(() => {
        if (prevValueRef.current === null && !waitingRef.current) {
          setMemoryLog(prev => { saveToLog(displayRef.current, prev); return prev; });
        }
      }, 5000);
    }
    return () => { if (digitTimerRef.current) clearTimeout(digitTimerRef.current); };
  }, [display, isFirstNumber, saveToLog]);

  const calculate = (a, b, op) => {
    switch(op) {
      case "+": return a + b;
      case "-": return a - b;
      case "×": return a * b;
      case "÷": return b !== 0 ? a / b : 0;
      default: return b;
    }
  };

  const formatNum = (num) => {
    if (!isFinite(num)) return "Error";
    if (Math.abs(num) >= 1e10) return num.toExponential(3);
    const s = String(num);
    if (s.length > 9) return parseFloat(num.toPrecision(8)).toString();
    return s;
  };

  const handleNumber = (num) => {
    if (waitingForOperand) {
      setDisplay(String(num));
      setWaitingForOperand(false);
    } else {
      setDisplay(prev => {
        if (prev === "0") return String(num);
        if (prev.replace(/[^0-9.]/g, "").length >= 9) return prev;
        return prev + num;
      });
    }
  };

  const handleDecimal = () => {
    if (waitingForOperand) { setDisplay("0."); setWaitingForOperand(false); return; }
    setDisplay(prev => prev.includes(".") ? prev : prev + ".");
  };

  const handleDelete = () => {
    setDisplay(prev => prev.length > 1 ? prev.slice(0, -1) : "0");
  };

  const handleOperator = (op) => {
    const current = parseFloat(display);
    if (prevValue !== null && !waitingForOperand) {
      const result = calculate(prevValue, current, operator);
      setDisplay(formatNum(result));
      setPrevValue(result);
    } else {
      setPrevValue(current);
    }
    setOperator(op);
    setWaitingForOperand(true);
  };

  const handleEquals = () => {
    if (operator && prevValue !== null) {
      const result = calculate(prevValue, parseFloat(display), operator);
      setDisplay(formatNum(result));
      setPrevValue(null);
      setOperator(null);
      setWaitingForOperand(true);
    }
  };

  const handleClear = () => {
    setDisplay("0");
    setPrevValue(null);
    setOperator(null);
    setWaitingForOperand(false);
  };

  const handlePercent = () => {
    setDisplay(prev => String(parseFloat(prev) / 100));
  };

  const getDisplayText = () => {
    if (display.endsWith(".")) return display;
    const num = parseFloat(display);
    if (isNaN(num)) return display;
    if (display.includes(".")) {
      const [int, dec] = display.split(".");
      return parseInt(int).toLocaleString() + "." + dec;
    }
    return num.toLocaleString();
  };

  const getFontSize = () => {
    const len = display.replace(/,/g, "").length;
    if (len <= 6) return "88px";
    if (len <= 9) return "64px";
    return "44px";
  };

  const startHold = (key, dur, cb) => { holdTimerRef.current[key] = setTimeout(cb, dur); };
  const cancelHold = (key) => { clearTimeout(holdTimerRef.current[key]); delete holdTimerRef.current[key]; };

  const onEqualsDown = (e) => { e.preventDefault(); startHold("eq", 500, () => { if (activeMemory) { setRecallValue(activeMemory); setRecalling(true); setDisplay(activeMemory); } }); };
  const onEqualsUp = (e) => { e.preventDefault(); cancelHold("eq"); if (recalling) { setRecalling(false); setDisplay("0"); setPrevValue(null); setOperator(null); setWaitingForOperand(false); } else { handleEquals(); } };

  const onClearDown = (e) => { e.preventDefault(); startHold("ac", 1000, async () => { setMemoryLog([]); setActiveMemory(null); try { await window.storage.delete(STORAGE_KEY); await window.storage.delete(ACTIVE_KEY); } catch(e){} handleClear(); }); };
  const onClearUp = (e) => { e.preventDefault(); cancelHold("ac"); handleClear(); };

  const onPlusDown = (e) => { e.preventDefault(); startHold("plus", 700, () => setShowMemoryOverlay(true)); };
  const onPlusUp = (e) => { e.preventDefault(); cancelHold("plus"); if (!showMemoryOverlay) handleOperator("+"); };

  const formatTime = (ts) => {
    const d = new Date(ts);
    return d.toLocaleTimeString([], {hour:"2-digit",minute:"2-digit"}) + "  " + d.toLocaleDateString([], {month:"short",day:"numeric"});
  };

  const SF = "-apple-system, 'SF Pro Display', 'Helvetica Neue', sans-serif";
  const ORANGE = "#FF9500";
  const DARK = "#2D2D2D";
  const GRAY = "#A5A5A5";

  // Reusable button component with press state
  const Btn = ({ children, bg, color = "#fff", onTap, onDown, onUp, onLeave, style = {} }) => {
    const [pressed, setPressed] = useState(false);
    const pressedBg = bg === ORANGE ? "#E68600" : bg === GRAY ? "#BDBDBD" : "#484848";
    return (
      <div
        onPointerDown={(e) => { e.preventDefault(); setPressed(true); onDown?.(e); }}
        onPointerUp={(e) => { e.preventDefault(); setPressed(false); onTap?.(e); onUp?.(e); }}
        onPointerLeave={(e) => { setPressed(false); onLeave?.(e); }}
        onPointerCancel={(e) => { setPressed(false); onLeave?.(e); }}
        style={{
          background: pressed ? pressedBg : bg,
          color,
          borderRadius: "50%",
          display: "flex",
          alignItems: "center",
          justifyContent: "center",
          cursor: "pointer",
          WebkitTapHighlightColor: "transparent",
          userSelect: "none",
          fontFamily: SF,
          transition: "background 0.05s",
          touchAction: "none",
          ...style,
        }}
      >
        {children}
      </div>
    );
  };

  const numBtnStyle = { fontSize: "36px", fontWeight: "400" };
  const opBtnStyle = { fontSize: "38px", fontWeight: "300" };
  const grayBtnStyle = { fontSize: "28px", fontWeight: "500", color: "#000" };

  return (
    <div style={{ display: "flex", alignItems: "center", justifyContent: "center", minHeight: "100dvh", height: "100dvh", background: "#000", fontFamily: SF, userSelect: "none", WebkitUserSelect: "none", WebkitTouchCallout: "none", position: "fixed", inset: 0, overflow: "hidden" }}>
      <style>{`* { box-sizing: border-box; } html, body { margin: 0; padding: 0; background: #000; height: 100%; overflow: hidden; }`}</style>
      <div style={{ width: "min(390px, 100vw)", height: "100dvh", background: "#000", display: "flex", flexDirection: "column", justifyContent: "flex-end", position: "relative" }}>

        {/* Display */}
        <div style={{ padding: "0 24px 4px", textAlign: "right", display: "flex", alignItems: "flex-end", justifyContent: "flex-end", minHeight: "130px" }}>
          <div style={{ color: "white", fontSize: getFontSize(), fontWeight: "200", letterSpacing: "-2px", lineHeight: 1, fontFamily: SF }}>
            {recalling ? recallValue : getDisplayText()}
          </div>
        </div>

        {/* Grid */}
        <div style={{ padding: "0 16px 34px", display: "grid", gridTemplateColumns: "repeat(4, 1fr)", gridTemplateRows: "repeat(5, 1fr)", gap: "12px" }}>

          {/* Row 1 */}
          <Btn bg={GRAY} onTap={handleDelete} style={{ ...grayBtnStyle, aspectRatio:"1" }}>
            <svg width="28" height="22" viewBox="0 0 28 22" fill="none">
              <path d="M10 2L2 11L10 20H26V2H10Z" stroke="#000" strokeWidth="2" fill="none"/>
              <line x1="14" y1="7" x2="22" y2="15" stroke="#000" strokeWidth="2" strokeLinecap="round"/>
              <line x1="22" y1="7" x2="14" y2="15" stroke="#000" strokeWidth="2" strokeLinecap="round"/>
            </svg>
          </Btn>
          <Btn bg={GRAY} style={{ ...grayBtnStyle, aspectRatio:"1" }} onDown={onClearDown} onUp={onClearUp} onLeave={() => cancelHold("ac")}>AC</Btn>
          <Btn bg={GRAY} style={{ ...grayBtnStyle, aspectRatio:"1" }} onTap={handlePercent}>%</Btn>
          <Btn bg={ORANGE} style={{ ...opBtnStyle, aspectRatio:"1" }} onTap={() => handleOperator("÷")}>÷</Btn>

          {/* Row 2 */}
          {["7","8","9"].map(n => <Btn key={n} bg={DARK} style={{ ...numBtnStyle, aspectRatio:"1" }} onTap={() => handleNumber(n)}>{n}</Btn>)}
          <Btn bg={ORANGE} style={{ ...opBtnStyle, aspectRatio:"1" }} onTap={() => handleOperator("×")}>×</Btn>

          {/* Row 3 */}
          {["4","5","6"].map(n => <Btn key={n} bg={DARK} style={{ ...numBtnStyle, aspectRatio:"1" }} onTap={() => handleNumber(n)}>{n}</Btn>)}
          <Btn bg={ORANGE} style={{ ...opBtnStyle, aspectRatio:"1" }} onTap={() => handleOperator("-")}>−</Btn>

          {/* Row 4 */}
          {["1","2","3"].map(n => <Btn key={n} bg={DARK} style={{ ...numBtnStyle, aspectRatio:"1" }} onTap={() => handleNumber(n)}>{n}</Btn>)}
          <Btn bg={ORANGE} style={{ ...opBtnStyle, aspectRatio:"1" }} onDown={onPlusDown} onUp={onPlusUp} onLeave={() => cancelHold("plus")}>+</Btn>

          {/* Row 5: wide 0, dot, = */}
          {/* Wide 0 spans 2 columns */}
          <div style={{ gridColumn: "1 / 3", position: "relative" }}>
            <div
              onPointerDown={(e) => e.preventDefault()}
              onPointerUp={(e) => { e.preventDefault(); handleNumber("0"); }}
              style={{
                background: DARK, color: "white", borderRadius: "40px",
                height: "100%", minHeight: "80px",
                display: "flex", alignItems: "center", justifyContent: "flex-start",
                paddingLeft: "30px", fontSize: "36px", fontWeight: "400",
                cursor: "pointer", userSelect: "none",
                WebkitTapHighlightColor: "transparent",
                fontFamily: SF, touchAction: "none",
              }}
            >
              0
              <span style={{
                position: "absolute", top: "10px", right: "12px",
                width: "4px", height: "4px", borderRadius: "50%",
                background: activeMemory ? "#5a5a5a" : "transparent",
                transition: "background 1.5s ease",
                pointerEvents: "none",
              }} />
            </div>
          </div>

          <Btn bg={DARK} style={{ ...numBtnStyle, fontWeight: "600", aspectRatio:"1" }} onTap={handleDecimal}>·</Btn>

          {/* = */}
          <div
            onPointerDown={onEqualsDown}
            onPointerUp={onEqualsUp}
            onPointerLeave={(e) => { cancelHold("eq"); if (recalling) { setRecalling(false); setDisplay("0"); } }}
            onContextMenu={(e) => e.preventDefault()}
            style={{
              background: ORANGE, borderRadius: "50%", aspectRatio: "1",
              display: "flex", alignItems: "center", justifyContent: "center",
              fontSize: "38px", fontWeight: "300", color: "white",
              cursor: "pointer",
              userSelect: "none",
              WebkitUserSelect: "none",
              MozUserSelect: "none",
              msUserSelect: "none",
              WebkitTapHighlightColor: "transparent",
              WebkitTouchCallout: "none",
              fontFamily: SF, touchAction: "none",
            }}
          >
            =
          </div>
        </div>

        {/* Recall label */}
        {recalling && (
          <div style={{ position: "absolute", top: 40, left: 0, right: 0, display: "flex", justifyContent: "center", pointerEvents: "none" }}>
            <div style={{ background: ORANGE, color: "#000", borderRadius: "8px", padding: "3px 10px", fontSize: "10px", fontWeight: "700", letterSpacing: "0.5px" }}>
              HOLD · RELEASE TO DISMISS
            </div>
          </div>
        )}

        {/* Memory log overlay */}
        {showMemoryOverlay && (
          <div style={{ position: "absolute", inset: 0, background: "rgba(0,0,0,0.97)", display: "flex", flexDirection: "column", zIndex: 100 }}>
            <div style={{ padding: "52px 22px 12px", display: "flex", justifyContent: "space-between", alignItems: "center" }}>
              <span style={{ color: "#555", fontSize: "11px", letterSpacing: "2px", textTransform: "uppercase" }}>Memory Log</span>
              <div onPointerUp={() => setShowMemoryOverlay(false)}
                style={{ background: "#1a1a1a", border: "none", color: "#888", borderRadius: "50%", width: "30px", height: "30px", cursor: "pointer", display: "flex", alignItems: "center", justifyContent: "center", fontSize: "14px" }}>
                ✕
              </div>
            </div>
            <div style={{ flex: 1, overflowY: "auto", padding: "0 22px" }}>
              {memoryLog.length === 0
                ? <div style={{ color: "#2a2a2a", textAlign: "center", marginTop: "80px", fontSize: "15px" }}>No entries logged yet</div>
                : memoryLog.map(e => (
                  <div key={e.id} onPointerUp={() => { setDisplay(e.value); setWaitingForOperand(false); setShowMemoryOverlay(false); }}
                    style={{ borderBottom: "1px solid #111", padding: "16px 0", display: "flex", justifyContent: "space-between", alignItems: "center", cursor: "pointer" }}>
                    <span style={{ color: "white", fontSize: "30px", fontWeight: "200", letterSpacing: "-1px" }}>{e.value}</span>
                    <span style={{ color: "#3a3a3a", fontSize: "11px" }}>{formatTime(e.timestamp)}</span>
                  </div>
                ))
              }
            </div>
            <div style={{ padding: "16px 22px 44px", color: "#222", fontSize: "10px", textAlign: "center", letterSpacing: "1px", textTransform: "uppercase" }}>
              Tap entry to use · Hold AC to clear all
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
