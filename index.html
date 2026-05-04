import React, { useEffect, useRef, useState } from 'react';

const brainwaveStates = [
  {
    label: 'Delta',
    description: 'Deep Sleep',
    frequencyRange: [0.5, 4],
    theme: {
      accent: '#3d6ea8',
      accentSoft: 'rgba(61, 110, 168, 0.10)',
      glow: 'rgba(61, 110, 168, 0.55)',
      panel: '#080d14',
      activePanel: '#0d1724',
      border: '#1c314a',
    },
  },
  {
    label: 'Theta',
    description: 'Relaxation',
    frequencyRange: [4, 8],
    theme: {
      accent: '#6a7fc6',
      accentSoft: 'rgba(106, 127, 198, 0.10)',
      glow: 'rgba(106, 127, 198, 0.55)',
      panel: '#0b0c15',
      activePanel: '#12142a',
      border: '#2a315f',
    },
  },
  {
    label: 'Alpha',
    description: 'Calm Focus',
    frequencyRange: [8, 12],
    theme: {
      accent: '#61a979',
      accentSoft: 'rgba(97, 169, 121, 0.10)',
      glow: 'rgba(97, 169, 121, 0.55)',
      panel: '#0a100d',
      activePanel: '#101b14',
      border: '#274934',
    },
  },
  {
    label: 'Beta',
    description: 'Alertness',
    frequencyRange: [12, 30],
    theme: {
      accent: '#e3a15e',
      accentSoft: 'rgba(227, 161, 94, 0.10)',
      glow: 'rgba(227, 161, 94, 0.55)',
      panel: '#100d09',
      activePanel: '#1b140c',
      border: '#5c4430',
    },
  },
  {
    label: 'Gamma',
    description: 'High Focus',
    frequencyRange: [30, 50],
    theme: {
      accent: '#d24b5f',
      accentSoft: 'rgba(210, 75, 95, 0.11)',
      glow: 'rgba(210, 75, 95, 0.62)',
      panel: '#12080b',
      activePanel: '#211016',
      border: '#74323d',
    },
  },
];

const clamp = (value, min, max) => Math.min(max, Math.max(min, value));
const carrierMinHz = 50;
const carrierMaxHz = 350;
const carrierGuideHz = [50, 63, 80, 100, 125, 160, 200, 250, 315, 350];

function yForCarrierHz(freq, height) {
  const minLog = Math.log(carrierMinHz);
  const maxLog = Math.log(carrierMaxHz);
  const normalized = (Math.log(freq) - minLog) / (maxLog - minLog);
  return height * (1 - normalized);
}

function getFrequenciesFromPoint(x, y, width, height, brainwave) {
  const safeWidth = Math.max(1, width);
  const safeHeight = Math.max(1, height);
  const normalizedX = clamp(x / safeWidth, 0, 1);
  const normalizedY = clamp(y / safeHeight, 0, 1);
  const minLog = Math.log(carrierMinHz);
  const maxLog = Math.log(carrierMaxHz);
  const baseFreq = Math.exp(minLog + (1 - normalizedY) * (maxLog - minLog));
  const [minBeat, maxBeat] = brainwave.frequencyRange;
  const beatFreq = minBeat + normalizedX * (maxBeat - minBeat);

  return {
    left: baseFreq,
    right: baseFreq + beatFreq,
    beat: beatFreq,
  };
}

export default function BinauralBeatVisuals() {
  const canvasRef = useRef(null);
  const audioCtxRef = useRef(null);
  const leftOscillatorRef = useRef(null);
  const rightOscillatorRef = useRef(null);
  const masterGainRef = useRef(null);
  const animationRef = useRef(null);
  const pointerRef = useRef({ x: 0, y: 0 });
  const lockedRef = useRef(false);
  const selectedBrainwaveRef = useRef(brainwaveStates[0]);
  const beatRef = useRef(2);
  const emdrModeRef = useRef(true);
  const sweepRateRef = useRef(1.0);

  const [selectedBrainwave, setSelectedBrainwave] = useState(brainwaveStates[0]);
  const [frequencies, setFrequencies] = useState({ left: 200, right: 202, beat: 2 });
  const [isLocked, setIsLocked] = useState(false);
  const [isPlaying, setIsPlaying] = useState(false);
  const [emdrMode, setEmdrMode] = useState(true);
  const [sweepRate, setSweepRate] = useState(1.0);

  const theme = selectedBrainwave.theme;

  const applyFrequencies = (x, y) => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    const next = getFrequenciesFromPoint(x, y, canvas.width, canvas.height, selectedBrainwaveRef.current);
    setFrequencies(next);
    beatRef.current = next.beat;

    if (leftOscillatorRef.current && rightOscillatorRef.current && audioCtxRef.current) {
      const now = audioCtxRef.current.currentTime;
      leftOscillatorRef.current.frequency.setTargetAtTime(next.left, now, 0.12);
      rightOscillatorRef.current.frequency.setTargetAtTime(next.right, now, 0.35);
    }
  };

  useEffect(() => {
    selectedBrainwaveRef.current = selectedBrainwave;
    applyFrequencies(pointerRef.current.x, pointerRef.current.y);
  }, [selectedBrainwave]);

  useEffect(() => {
    lockedRef.current = isLocked;
  }, [isLocked]);

  useEffect(() => {
    emdrModeRef.current = emdrMode;
  }, [emdrMode]);

  useEffect(() => {
    sweepRateRef.current = sweepRate;
  }, [sweepRate]);

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');

    const resize = () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      if (pointerRef.current.x === 0 && pointerRef.current.y === 0) {
        pointerRef.current = { x: canvas.width / 2, y: canvas.height / 2 };
      } else {
        pointerRef.current = {
          x: clamp(pointerRef.current.x, 0, canvas.width),
          y: clamp(pointerRef.current.y, 0, canvas.height),
        };
      }
      applyFrequencies(pointerRef.current.x, pointerRef.current.y);
    };

    const draw = () => {
      const { x, y } = pointerRef.current;
      const activeTheme = selectedBrainwaveRef.current.theme;
      const nowSeconds = performance.now() / 1000;
      const beat = Math.max(0.001, beatRef.current);
      const sweepRateNow = sweepRateRef.current;
      const sweep = 0.5 + 0.5 * Math.sin(nowSeconds * sweepRateNow * Math.PI * 2 - Math.PI / 2);
      const margin = Math.max(72, canvas.width * 0.08);
      const pingX = margin + sweep * Math.max(1, canvas.width - margin * 2);
      const pingY = canvas.height * 0.5;
      const pulse = 0.5 + 0.5 * Math.sin(nowSeconds * beat * Math.PI * 2);

      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = '#070708';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      ctx.strokeStyle = activeTheme.accentSoft;
      ctx.lineWidth = 1;
      const verticalGrid = 48;
      for (let gx = 0; gx < canvas.width; gx += verticalGrid) {
        ctx.beginPath();
        ctx.moveTo(gx, 0);
        ctx.lineTo(gx, canvas.height);
        ctx.stroke();
      }

      carrierGuideHz.forEach((freq) => {
        const gy = yForCarrierHz(freq, canvas.height);
        const isOctaveMarker = freq === 50 || freq === 100 || freq === 200;
        ctx.strokeStyle = isOctaveMarker
          ? activeTheme.accentSoft.replace('0.10', '0.22').replace('0.11', '0.22')
          : activeTheme.accentSoft;
        ctx.lineWidth = isOctaveMarker ? 1.25 : 1;
        ctx.beginPath();
        ctx.moveTo(0, gy);
        ctx.lineTo(canvas.width, gy);
        ctx.stroke();

        ctx.fillStyle = isOctaveMarker ? activeTheme.accent : '#5f5a50';
        ctx.font = '9px Courier, monospace';
        ctx.textAlign = 'right';
        ctx.fillText(`${freq}Hz`, canvas.width - 10, gy - 4);
      });

      if (emdrModeRef.current) {
        ctx.strokeStyle = activeTheme.accentSoft;
        ctx.globalAlpha = 0.16;
        ctx.lineWidth = 1;
        ctx.beginPath();
        ctx.moveTo(margin, pingY);
        ctx.lineTo(canvas.width - margin, pingY);
        ctx.stroke();

        const glow = ctx.createRadialGradient(pingX, pingY, 0, pingX, pingY, 24 + pulse * 12);
        glow.addColorStop(0, activeTheme.glow.replace('0.55', '0.65').replace('0.62', '0.7'));
        glow.addColorStop(0.35, activeTheme.glow.replace('0.55', '0.28').replace('0.62', '0.32'));
        glow.addColorStop(1, 'rgba(0,0,0,0)');
        ctx.fillStyle = glow;
        ctx.globalAlpha = 1;
        ctx.beginPath();
        ctx.arc(pingX, pingY, 24 + pulse * 12, 0, Math.PI * 2);
        ctx.fill();

        ctx.fillStyle = activeTheme.accent;
        ctx.shadowBlur = 14;
        ctx.shadowColor = activeTheme.glow;
        ctx.beginPath();
        ctx.arc(pingX, pingY, 4 + pulse * 2, 0, Math.PI * 2);
        ctx.fill();
        ctx.shadowBlur = 0;
      }

      ctx.strokeStyle = activeTheme.accent;
      ctx.lineWidth = 1.5;
      ctx.shadowBlur = lockedRef.current ? 10 : 4;
      ctx.shadowColor = activeTheme.glow;
      ctx.beginPath();
      ctx.arc(x, y, 15, Math.PI * 0.62, Math.PI * 1.38);
      ctx.stroke();
      ctx.beginPath();
      ctx.arc(x, y, 15, Math.PI * 1.62, Math.PI * 2.38);
      ctx.stroke();

      ctx.shadowBlur = 0;
      ctx.fillStyle = activeTheme.accent;
      ctx.beginPath();
      ctx.arc(x, y, 4, 0, Math.PI * 2);
      ctx.fill();
      ctx.globalAlpha = 1;

      animationRef.current = requestAnimationFrame(draw);
    };

    const handlePointerMove = (event) => {
      if (lockedRef.current) return;
      pointerRef.current = { x: event.clientX, y: event.clientY };
      applyFrequencies(event.clientX, event.clientY);
    };

    const handleClick = (event) => {
      pointerRef.current = { x: event.clientX, y: event.clientY };
      applyFrequencies(event.clientX, event.clientY);
      setIsLocked((previous) => !previous);
    };

    resize();
    draw();
    window.addEventListener('resize', resize);
    canvas.addEventListener('pointermove', handlePointerMove);
    canvas.addEventListener('click', handleClick);

    return () => {
      window.removeEventListener('resize', resize);
      canvas.removeEventListener('pointermove', handlePointerMove);
      canvas.removeEventListener('click', handleClick);
      cancelAnimationFrame(animationRef.current);
    };
  }, []);

  const toggleAudio = async () => {
    const AudioContextClass = window.AudioContext || window.webkitAudioContext;

    if (!audioCtxRef.current) {
      const audioCtx = new AudioContextClass();
      const leftOscillator = audioCtx.createOscillator();
      const rightOscillator = audioCtx.createOscillator();
      const leftGain = audioCtx.createGain();
      const rightGain = audioCtx.createGain();
      const merger = audioCtx.createChannelMerger(2);
      const master = audioCtx.createGain();

      leftOscillator.type = 'sine';
      rightOscillator.type = 'sine';
      leftGain.gain.value = 0.08;
      rightGain.gain.value = 0.08;
      master.gain.value = 0;

      leftOscillator.connect(leftGain);
      rightOscillator.connect(rightGain);
      leftGain.connect(merger, 0, 0);
      rightGain.connect(merger, 0, 1);
      merger.connect(master);
      master.connect(audioCtx.destination);

      leftOscillator.start();
      rightOscillator.start();

      audioCtxRef.current = audioCtx;
      leftOscillatorRef.current = leftOscillator;
      rightOscillatorRef.current = rightOscillator;
      masterGainRef.current = master;

      applyFrequencies(pointerRef.current.x, pointerRef.current.y);

      const now = audioCtx.currentTime;
      master.gain.cancelScheduledValues(now);
      master.gain.setValueAtTime(0, now);
      master.gain.linearRampToValueAtTime(0.75, now + 2);
      setIsPlaying(true);
      return;
    }

    const now = audioCtxRef.current.currentTime;
    const currentGain = masterGainRef.current.gain.value;
    masterGainRef.current.gain.cancelScheduledValues(now);
    masterGainRef.current.gain.setValueAtTime(currentGain, now);

    if (isPlaying) {
      masterGainRef.current.gain.linearRampToValueAtTime(0, now + 3);
      setIsPlaying(false);
    } else {
      if (audioCtxRef.current.state !== 'running') await audioCtxRef.current.resume();
      masterGainRef.current.gain.linearRampToValueAtTime(0.75, now + 2);
      setIsPlaying(true);
    }
  };

  const toggleEmdr = (event) => {
    event.stopPropagation();
    const next = !emdrModeRef.current;
    emdrModeRef.current = next;
    setEmdrMode(next);
  };

  const currentRange = selectedBrainwave.frequencyRange;

  return (
    <div className="w-screen h-screen overflow-hidden bg-[#070708] text-[#c9c3b5] font-mono select-none">
      <canvas ref={canvasRef} className="absolute inset-0 cursor-crosshair" />

      <img
        src="/mnt/data/ebm.jpg"
        alt="EBM Logo"
        className="absolute inset-0 m-auto w-[320px] opacity-[0.06] pointer-events-none"
        style={{ filter: 'contrast(1.1)' }}
      />

      <div className="absolute top-0 left-0 right-0 flex border-b bg-[#09090b]" style={{ borderColor: theme.border }}>
        <button
          onClick={toggleAudio}
          className="border-r px-4 py-3 text-[10px] tracking-[0.16em] uppercase"
          style={{ borderColor: '#161616', color: isPlaying ? theme.accent : '#555', background: isPlaying ? theme.activePanel : 'transparent' }}
        >
          Audio
        </button>

        <button
          onClick={toggleEmdr}
          className="border-r px-4 py-3 text-[10px] tracking-[0.16em] uppercase"
          style={{
            borderColor: '#161616',
            color: emdrMode ? theme.accent : '#555',
            background: emdrMode ? theme.activePanel : 'transparent',
            boxShadow: emdrMode ? `inset 0 -2px 0 ${theme.accent}` : 'none',
          }}
        >
          EMDR Mode
        </button>

        <label className="flex w-[190px] items-center gap-2 border-r px-3 py-2" style={{ borderColor: '#161616' }}>
          <span className="text-[8px] uppercase tracking-[0.16em] text-[#8f8a7b]">Speed</span>
          <input
            type="range"
            min="0.3"
            max="3"
            step="0.01"
            value={sweepRate}
            onChange={(event) => setSweepRate(parseFloat(event.target.value))}
            className="w-full accent-current"
            style={{ color: theme.accent }}
          />
        </label>

        <div className="flex min-w-0 flex-1 items-center gap-3 border-r px-4 py-2" style={{ borderColor: '#161616' }}>
          <span className="text-[8px] uppercase tracking-[0.16em] text-[#8f8a7b]">Left / Right</span>
          <span className="text-[13px] font-bold tracking-[0.12em]" style={{ color: theme.accent }}>
            {frequencies.left.toFixed(1)}Hz / {frequencies.right.toFixed(1)}Hz
          </span>
        </div>

        <div className="flex items-center gap-3 px-4 py-2">
          <span className="text-[8px] uppercase tracking-[0.16em] text-[#8f8a7b]">Beat</span>
          <span className="text-[13px] font-bold tracking-[0.12em]" style={{ color: theme.accent }}>
            {frequencies.beat.toFixed(2)}Hz
          </span>
        </div>
      </div>

      <div className="absolute left-3 top-16 w-[190px] border p-2 shadow-[0_0_24px_rgba(0,0,0,0.45)]" style={{ background: `${theme.panel}f2`, borderColor: theme.border }}>
        <div className="mb-2 border-b pb-2 text-[8px] uppercase tracking-[0.18em] text-[#8f8a7b]" style={{ borderColor: '#181818' }}>
          Brainwave State
        </div>

        <div className="grid gap-[1px] bg-[#121212]">
          {brainwaveStates.map((nextState) => {
            const active = selectedBrainwave.label === nextState.label;
            return (
              <button
                key={nextState.label}
                onClick={(event) => {
                  event.stopPropagation();
                  setSelectedBrainwave(nextState);
                }}
                className="flex items-center justify-between border-0 px-3 py-3 text-left"
                style={{
                  background: active ? nextState.theme.activePanel : '#0d0d10',
                  color: active ? nextState.theme.accent : '#777',
                  boxShadow: active ? `inset 0 -2px 0 ${nextState.theme.accent}` : 'none',
                }}
              >
                <span className="text-[11px] font-bold uppercase tracking-[0.12em]">{nextState.label}</span>
                <span className="text-[8px] uppercase tracking-[0.1em] text-[#8f8a7b]">{nextState.frequencyRange[0]}–{nextState.frequencyRange[1]}Hz</span>
              </button>
            );
          })}
        </div>

        <div className="mt-3 border-t pt-3" style={{ borderColor: '#181818' }}>
          <div className="mb-1 text-[8px] uppercase tracking-[0.18em] text-[#8f8a7b]">Current</div>
          <div className="text-[14px] font-bold tracking-[0.08em]" style={{ color: theme.accent }}>
            {frequencies.left.toFixed(1)}Hz / {frequencies.right.toFixed(1)}Hz
          </div>
          <div className="mt-1 text-[9px] uppercase tracking-[0.1em] text-[#5f5a50]">
            {selectedBrainwave.description} · {currentRange[0]}–{currentRange[1]}Hz
          </div>
        </div>

        <div className="mt-3 border-t pt-3 text-[9px] uppercase tracking-[0.12em] text-[#5f5a50]" style={{ borderColor: '#181818' }}>
          {isLocked ? 'Cursor Locked' : 'Cursor Live'} · Sweep {sweepRate.toFixed(2)}Hz
        </div>
      </div>

      <div className="absolute bottom-0 left-0 right-0 flex border-t bg-[#08080a] text-[9px] uppercase tracking-[0.13em] text-[#777]" style={{ borderColor: theme.border }}>
        <div className="flex-1 border-r px-4 py-3" style={{ borderColor: '#181818' }}>Y Axis: Carrier Pitch</div>
        <div className="flex-1 border-r px-4 py-3" style={{ borderColor: '#181818' }}>X Axis: Beat Within State</div>
        <div className="flex-1 px-4 py-3">EMDR Mode: Light Ball On / Off</div>
      </div>
    </div>
  );
}
