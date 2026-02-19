import React, { useEffect, useMemo, useRef, useState } from "react";
import { MapContainer, TileLayer, CircleMarker, Circle, useMap } from "react-leaflet";
import "leaflet/dist/leaflet.css";

type Mode = "wide" | "near" | "3m";
type Spot = { id: string; lat: number; lng: number; mbti: string };

const MBTIS = [
  "INTJ","INTP","ENTJ","ENTP",
  "INFJ","INFP","ENFJ","ENFP",
  "ISTJ","ISFJ","ESTJ","ESFJ",
  "ISTP","ISFP","ESTP","ESFP"
];

const clamp = (n: number, a: number, b: number) => Math.max(a, Math.min(b, n));

function distanceMeters(aLat: number, aLng: number, bLat: number, bLng: number) {
  const R = 6371000;
  const toRad = (d: number) => (d * Math.PI) / 180;
  const dLat = toRad(bLat - aLat);
  const dLng = toRad(bLng - aLng);
  const lat1 = toRad(aLat);
  const lat2 = toRad(bLat);
  const x =
    Math.sin(dLat / 2) ** 2 +
    Math.cos(lat1) * Math.cos(lat2) * Math.sin(dLng / 2) ** 2;
  return 2 * R * Math.asin(Math.sqrt(x));
}

function vibeScore(a: string, b: string) {
  let s = 0;
  for (let i = 0; i < 4; i++) if (a[i] === b[i]) s += 1;
  return s;
}

function rand(min: number, max: number) {
  return min + Math.random() * (max - min);
}

function generateSpots(lat: number, lng: number, count: number): Spot[] {
  const spread = 0.007;
  return Array.from({ length: count }).map((_, i) => ({
    id: `spot_${i}_${Math.random().toString(16).slice(2)}`,
    lat: lat + rand(-spread, spread),
    lng: lng + rand(-spread, spread),
    mbti: MBTIS[Math.floor(Math.random() * MBTIS.length)],
  }));
}

function Recenter({ lat, lng, zoom }: { lat: number; lng: number; zoom: number }) {
  const map = useMap();
  useEffect(() => {
    map.setView([lat, lng], zoom, { animate: true });
  }, [lat, lng, zoom, map]);
  return null;
}

function Toast({ show, title, sub }: { show: boolean; title: string; sub?: string }) {
  return (
    <div
      style={{
        position: "absolute",
        top: 14,
        left: 14,
        right: 14,
        zIndex: 9999,
        display: "flex",
        justifyContent: "center",
        pointerEvents: "none",
        transform: show ? "translateY(0)" : "translateY(-12px)",
        opacity: show ? 1 : 0,
        transition: "all 220ms ease",
      }}
    >
      <div
        style={{
          width: "min(560px, 100%)",
          background: "rgba(10,10,12,0.82)",
          border: "1px solid rgba(255,255,255,0.10)",
          borderRadius: 16,
          padding: "12px 14px",
          boxShadow: "0 10px 30px rgba(0,0,0,0.35)",
          backdropFilter: "blur(10px)",
          color: "white",
        }}
      >
        <div style={{ fontSize: 15, fontWeight: 900, letterSpacing: "-0.2px" }}>{title}</div>
        {sub ? <div style={{ marginTop: 4, fontSize: 13, opacity: 0.85, lineHeight: 1.25 }}>{sub}</div> : null}
      </div>
    </div>
  );
}

export default function MVP_SPOT() {
  // ✅ (3) 클라이언트에서만 맵 렌더 가드
  const [mounted, setMounted] = useState(false);
  useEffect(() => setMounted(true), []);

  const [mode, setMode] = useState<Mode>("wide");
  const [myMbti, setMyMbti] = useState(() => MBTIS[Math.floor(Math.random() * MBTIS.length)]);

  const [me, setMe] = useState({ lat: 37.5665, lng: 126.9780 });
  const [spots, setSpots] = useState<Spot[]>(() => generateSpots(37.5665, 126.9780, 40));

  const [toast, setToast] = useState({ show: true, title: "지금 이 근처에…", sub: "나랑 비슷한 MBTI가 있을까?" });

  // ✅ (1) setTimeout 타입 안전 + cleanup
  const timer = useRef<ReturnType<typeof window.setTimeout> | null>(null);
  useEffect(() => {
    return () => {
      if (timer.current) window.clearTimeout(timer.current);
    };
  }, []);

  const zoomByMode: Record<Mode, number> = { wide: 13, near: 16, "3m": 18 };
  const is3m = mode === "3m";

  useEffect(() => {
    if (!navigator.geolocation) {
      setToast({ show: true, title: "위치 기능을 사용할 수 없어요.", sub: "브라우저에서 위치 권한을 확인해줘." });
      return;
    }

    navigator.geolocation.getCurrentPosition(
      (pos) => {
        const lat = pos.coords.latitude;
        const lng = pos.coords.longitude;
        setMe({ lat, lng });
        setSpots(generateSpots(lat, lng, 40));
      },
      // ✅ (2) 실패 안내 추가
      (err) => {
        setToast({
          show: true,
          title: "위치를 못 가져왔어.",
          sub: err.code === err.PERMISSION_DENIED ? "위치 권한을 허용해줘." : "네트워크/권한 상태를 확인해줘.",
        });
      },
      { enableHighAccuracy: true, timeout: 8000 }
    );
  }, []);

  function showToast(title: string, sub?: string, ms = 1500) {
    setToast({ show: true, title, sub });
    if (timer.current) window.clearTimeout(timer.current);
    timer.current = window.setTimeout(() => setToast((t) => ({ ...t, show: false })), ms);
  }

  function changeMode(next: Mode) {
    setMode(next);
    if (next === "wide") showToast("오늘의 흐름이 보여.", "어디로 모였는지.");
    if (next === "near") showToast("가까워질수록 또렷해져.", "비슷한 쪽이 먼저 보여.");
    if (next === "3m") {
      showToast("3m 안. 이 골목 어딘가에.", "지금, 마주칠 수도.");
      if (navigator.vibrate) navigator.vibrate([40, 30, 40]);
    }
  }

  const visibleSpots = useMemo(() => {
    if (mode === "wide") return spots;

    const scored = spots
      .map((s) => ({ s, score: vibeScore(myMbti, s.mbti) }))
      .sort((a, b) => b.score - a.score);

    let filtered = scored.filter((x) => x.score >= 3).map((x) => x.s);
    if (filtered.length < 10) filtered = scored.filter((x) => x.score >= 2).map((x) => x.s);

    if (mode === "3m") return filtered.slice(0, 5);
    return filtered.slice(0, 18);
  }, [mode, spots, myMbti]);

  function markerStyle(s: Spot) {
    const d = distanceMeters(me.lat, me.lng, s.lat, s.lng);
    const maxD = mode === "wide" ? 2000 : 600;
    const t = 1 - clamp(d / maxD, 0, 1);
    const opacity = 0.10 + t * 0.80;
    const radius = 5 + t * 10;
    const hue = (s.mbti.charCodeAt(0) + s.mbti.charCodeAt(3)) % 360;
    const color = `hsl(${hue}, 82%, 62%)`;
    return { opacity, radius, color, d };
  }

  const pageBg =
    "radial-gradient(1200px 700px at 50% 0%, rgba(255,255,255,0.08), rgba(0,0,0,0) 55%), #07070A";

  // ✅ mounted 전에는 맵 렌더하지 않음 (SSR/프리렌더/테스트 안정)
  if (!mounted) {
    return (
      <div style={{ height: "100vh", width: "100vw", background: pageBg, display: "grid", placeItems: "center", color: "white" }}>
        <div style={{ opacity: 0.8, fontWeight: 900 }}>Loading map…</div>
      </div>
    );
  }

  return (
    <div style={{ height: "100vh", width: "100vw", background: pageBg, position: "relative", color: "white" }}>
      <Toast show={toast.show} title={toast.title} sub={toast.sub} />
      {/* ... 이하 UI/Map 코드 동일 ... */}
      {/* 너 원본 그대로 두면 됨 */}
    </div>
  );
}
