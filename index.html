import { useState, useMemo, useEffect, useCallback } from "react";
import { PieChart, Pie, Cell, BarChart, Bar, XAxis, YAxis, Tooltip, ResponsiveContainer, Treemap } from "recharts";

// ─── Claude Brand Palette ────────────────────────────────────────────────────
const AGING_KEYS = [
  { key: 'current',  label: 'Current', color: '#5C8A6E' },
  { key: 'd30',     label: '1–30',    color: '#C8935A' },
  { key: 'd60',     label: '31–60',   color: '#D97757' },
  { key: 'd90',     label: '61–90',   color: '#C05A3A' },
  { key: 'd120',    label: '91–120',  color: '#8B3A22' },
  { key: 'over120', label: '120+',    color: '#4A1E0F' },
];

const fmt = (v) => {
  if (v >= 1e9) return `₱${(v/1e9).toFixed(2)}B`;
  if (v >= 1e6) return `₱${(v/1e6).toFixed(2)}M`;
  if (v >= 1e3) return `₱${(v/1e3).toFixed(0)}K`;
  return `₱${v.toFixed(0)}`;
};
const fmtFull = (v) => `₱${v.toLocaleString('en-US',{minimumFractionDigits:2,maximumFractionDigits:2})}`;

const getUrgency = (s) => {
  const critical = s.d90 + s.d120 + s.over120;
  if (critical > 0 && critical / s.total > 0.2) return 'CRITICAL';
  if (s.d60 > 0 || critical > 0) return 'ATTENTION';
  return 'OK';
};

const URGENCY_INFO = {
  CRITICAL:  { bg: '#FDF0EB', border: '#F0C4AF', text: '#7A2E10', badge: '#C05A3A', badgeText: '#fff', label: 'CRITICAL',  desc: 'Over 20% of payables are 61+ days past due. Escalate for payment approval.' },
  ATTENTION: { bg: '#FDF5ED', border: '#EDD5B0', text: '#7A4E1A', badge: '#C8935A', badgeText: '#fff', label: 'ATTENTION', desc: 'Overdue balances in 31–60 day range or smaller 61+ amounts. Schedule payment soon.' },
  OK:        { bg: '#EEF4F0', border: '#BDD5C6', text: '#2D5940', badge: '#5C8A6E', badgeText: '#fff', label: 'ON TRACK',  desc: 'All balances are current. Maintain standard payment cycle.' },
};

const C = {
  brand:      '#D97757',
  bg:         '#FAF9F7',
  surface:    '#FFFFFF',
  border:     '#EDE9E3',
  textDark:   '#1A1714',
  textMid:    '#6B6358',
  textMute:   '#A89F94',
  tableStripe:'#FAF7F4',
};

// ─── GitHub raw URL — update this when you push a new data file ──────────────
const GITHUB_DATA_URL = 'https://raw.githubusercontent.com/YOUR_ORG/YOUR_REPO/main/ap_data.json';

const UrgencyBadge = ({ level }) => {
  const s = URGENCY_INFO[level];
  return <span style={{ background: s.badge, color: s.badgeText, padding: '3px 10px', borderRadius: 4, fontSize: 10, fontWeight: 700, letterSpacing: '0.07em' }}>{s.label}</span>;
};

const CustomTooltipBar = ({ active, payload, label }) => {
  if (!active || !payload?.length) return null;
  return (
    <div style={{ background: C.surface, border: `1px solid ${C.border}`, borderRadius: 8, padding: '10px 14px', fontSize: 12, boxShadow: '0 4px 16px rgba(90,60,40,0.10)' }}>
      <p style={{ color: C.textDark, fontWeight: 600, marginBottom: 4, maxWidth: 200, whiteSpace: 'nowrap', overflow: 'hidden', textOverflow: 'ellipsis' }}>{label}</p>
      {payload.map((p, i) => (<p key={i} style={{ color: p.color, margin: '2px 0' }}>{p.name}: {fmtFull(p.value)}</p>))}
    </div>
  );
};

// ─── Main Dashboard ───────────────────────────────────────────────────────────
export default function APDashboard() {
  // ── Data state
  const [suppliers, setSuppliers] = useState([]);
  const [grand, setGrand] = useState(null);
  const [rawDocs, setRawDocs] = useState([]);
  const [meta, setMeta] = useState({ label: 'As of —', totalSuppliers: 0, totalInvoices: 0 });

  // ── Sync state
  const [syncStatus, setSyncStatus] = useState('idle'); // idle | loading | success | error
  const [syncError, setSyncError] = useState('');
  const [lastSynced, setLastSynced] = useState('');

  // ── UI state
  const [view, setView] = useState('overview');
  const [selectedSupplier, setSelectedSupplier] = useState(null);
  const [sortBy, setSortBy] = useState('total');
  const [filterUrgency, setFilterUrgency] = useState('ALL');
  const [searchTerm, setSearchTerm] = useState('');

  // ── Fetch from GitHub
  const fetchData = useCallback(async () => {
    setSyncStatus('loading');
    setSyncError('');
    try {
      const res = await fetch(GITHUB_DATA_URL, { cache: 'no-store' });
      if (!res.ok) throw new Error(`HTTP ${res.status}: ${res.statusText}`);
      const json = await res.json();
      if (!json.suppliers || !json.grand) throw new Error('Invalid data format. Expected keys: suppliers, grand, docs, meta.');
      setSuppliers(json.suppliers || []);
      setGrand(json.grand);
      setRawDocs(json.docs || []);
      setMeta(json.meta || { label: 'Fetched', totalSuppliers: json.suppliers.length, totalInvoices: (json.docs || []).length });
      setLastSynced(new Date().toLocaleString('en-PH', { dateStyle: 'medium', timeStyle: 'short' }));
      setSyncStatus('success');
      setTimeout(() => setSyncStatus('idle'), 3000);
    } catch (err) {
      setSyncStatus('error');
      setSyncError(err.message);
    }
  }, []);

  // Auto-fetch on mount
  useEffect(() => { fetchData(); }, []);

  // ── Derived data (safe defaults when not yet loaded)
  const overdueTotal  = grand ? grand.d30 + grand.d60 + grand.d90 + grand.d120 + grand.over120 : 0;
  const criticalTotal = grand ? grand.d90 + grand.d120 + grand.over120 : 0;
  const overduePercent = grand ? ((overdueTotal / grand.total) * 100).toFixed(1) : '0.0';
  const agingPieData = grand ? AGING_KEYS.map(a => ({ name: a.label, value: grand[a.key], color: a.color })).filter(d => d.value > 0) : [];

  const sortedSuppliers = useMemo(() => {
    let list = [...suppliers];
    if (filterUrgency !== 'ALL') list = list.filter(s => getUrgency(s) === filterUrgency);
    if (searchTerm) list = list.filter(s => s.name.toLowerCase().includes(searchTerm.toLowerCase()));
    list.sort((a, b) => {
      if (sortBy === 'total')   return b.total - a.total;
      if (sortBy === 'overdue') return (b.d30+b.d60+b.d90+b.d120+b.over120)-(a.d30+a.d60+a.d90+a.d120+a.over120);
      if (sortBy === 'urgency') { const r = { CRITICAL: 3, ATTENTION: 2, OK: 1 }; return r[getUrgency(b)]-r[getUrgency(a)]; }
      return b.invoices - a.invoices;
    });
    return list;
  }, [suppliers, sortBy, filterUrgency, searchTerm]);

  const top8Bar = [...suppliers].sort((a, b) => b.total - a.total).slice(0, 8);

  const treemapData = suppliers.filter(s => s.total > 5000000).map(s => ({
    name: s.name.length > 20 ? s.name.substring(0, 20) + '…' : s.name,
    size: s.total,
    color: getUrgency(s) === 'CRITICAL' ? '#DFA08A' : getUrgency(s) === 'ATTENTION' ? '#DFC08A' : '#8AB8A0',
  }));

  const selectedData = selectedSupplier ? suppliers.find(s => s.bp_code === selectedSupplier) : null;

  const navStyle = (v) => ({
    padding: '8px 20px', borderRadius: 7, border: 'none', cursor: 'pointer',
    fontSize: 13, fontWeight: 600, fontFamily: "'DM Sans', sans-serif",
    letterSpacing: '0.01em', transition: 'all 0.18s',
    background: view === v ? C.brand : 'transparent',
    color: view === v ? '#fff' : C.textMid,
    boxShadow: view === v ? '0 2px 8px rgba(217,119,87,0.32)' : 'none',
  });

  const card = {
    background: C.surface, border: `1px solid ${C.border}`,
    borderRadius: 12, padding: '20px 22px',
    boxShadow: '0 1px 3px rgba(90,60,40,0.05)',
  };

  const isLoading = syncStatus === 'loading';
  const isEmpty = !grand && !isLoading;

  return (
    <div style={{ minHeight: '100vh', background: C.bg, color: C.textDark, fontFamily: "'DM Sans', sans-serif" }}>
      <link href="https://fonts.googleapis.com/css2?family=DM+Sans:opsz,wght@9..40,400;9..40,500;9..40,600;9..40,700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />

      {/* ── Header ── */}
      <div style={{ background: C.surface, borderBottom: `1px solid ${C.border}`, padding: '14px 28px', display: 'flex', alignItems: 'center', justifyContent: 'space-between', position: 'sticky', top: 0, zIndex: 100, boxShadow: '0 1px 4px rgba(90,60,40,0.06)' }}>
        <div style={{ display: 'flex', alignItems: 'center', gap: 12 }}>
          <div style={{ width: 36, height: 36, borderRadius: 9, background: C.brand, display: 'flex', alignItems: 'center', justifyContent: 'center', fontSize: 13, fontWeight: 700, color: '#fff' }}>AP</div>
          <div>
            <h1 style={{ margin: 0, fontSize: 17, fontWeight: 700, color: C.textDark }}>GTC AP Monitoring</h1>
            <p style={{ margin: 0, fontSize: 11.5, color: C.textMute }}>
              {meta.label} · {meta.totalSuppliers} suppliers · {meta.totalInvoices} open entries
              {lastSynced && <span style={{ marginLeft: 8 }}>· Synced: {lastSynced}</span>}
            </p>
          </div>
        </div>

        <div style={{ display: 'flex', alignItems: 'center', gap: 10 }}>
          <button
            onClick={fetchData}
            disabled={isLoading}
            style={{
              display: 'flex', alignItems: 'center', gap: 6,
              padding: '7px 16px', borderRadius: 7, border: `1px solid ${C.border}`,
              background: syncStatus === 'success' ? '#EEF4F0' : syncStatus === 'error' ? '#FDF0EB' : C.surface,
              color: syncStatus === 'success' ? '#2D5940' : syncStatus === 'error' ? '#7A2E10' : C.textMid,
              fontSize: 12, fontWeight: 600, cursor: isLoading ? 'wait' : 'pointer',
              fontFamily: "'DM Sans', sans-serif", transition: 'all 0.2s',
            }}>
            <span style={{ fontSize: 14, display: 'inline-block', animation: isLoading ? 'spin 1s linear infinite' : 'none' }}>
              {isLoading ? '⟳' : syncStatus === 'success' ? '✓' : syncStatus === 'error' ? '⚠' : '↻'}
            </span>
            {isLoading ? 'Syncing…' : syncStatus === 'success' ? 'Synced' : syncStatus === 'error' ? 'Retry Sync' : 'Sync'}
          </button>

          <div style={{ display: 'flex', gap: 4, background: '#F2EDE8', borderRadius: 9, padding: 3 }}>
            {['overview','suppliers','payments'].map(v => (
              <button key={v} onClick={() => { setView(v); setSelectedSupplier(null); }} style={navStyle(v)}>
                {v === 'overview' ? 'Overview' : v === 'suppliers' ? 'Suppliers' : 'Payment Queue'}
              </button>
            ))}
          </div>
        </div>
      </div>

      {/* ── Error banner ── */}
      {syncStatus === 'error' && syncError && (
        <div style={{ background: '#FDF0EB', borderBottom: `1px solid #F0C4AF`, padding: '10px 28px', display: 'flex', alignItems: 'center', gap: 10 }}>
          <span style={{ color: '#C05A3A', fontSize: 13, fontWeight: 600 }}>⚠ Sync failed:</span>
          <span style={{ color: '#7A2E10', fontSize: 13 }}>{syncError}</span>
        </div>
      )}

      <div style={{ padding: '22px 28px', maxWidth: 1400, margin: '0 auto' }}>

        {/* ── Loading skeleton ── */}
        {isLoading && (
          <div style={{ display: 'grid', gridTemplateColumns: 'repeat(4, 1fr)', gap: 14, marginBottom: 20 }}>
            {[0,1,2,3].map(i => (
              <div key={i} style={{ ...card, height: 90, background: 'linear-gradient(90deg, #F5F0EB 25%, #EDE9E3 50%, #F5F0EB 75%)', backgroundSize: '200% 100%', animation: 'shimmer 1.4s infinite' }} />
            ))}
            <style>{`@keyframes shimmer{0%{background-position:200% 0}100%{background-position:-200% 0}} @keyframes spin{from{transform:rotate(0deg)}to{transform:rotate(360deg)}}`}</style>
          </div>
        )}

        {/* ── Empty state ── */}
        {isEmpty && !isLoading && (
          <div style={{ textAlign: 'center', padding: '80px 20px' }}>
            <div style={{ fontSize: 40, marginBottom: 16 }}>📂</div>
            <h2 style={{ margin: '0 0 8px', fontSize: 20, fontWeight: 700, color: C.textDark }}>No data loaded</h2>
            <p style={{ margin: '0 0 24px', fontSize: 14, color: C.textMute, maxWidth: 400, marginInline: 'auto' }}>
              Update <code style={{ background: '#F2EDE8', padding: '1px 6px', borderRadius: 3, fontSize: 13 }}>GITHUB_DATA_URL</code> in the file and click Sync.
            </p>
            <button onClick={fetchData} style={{ padding: '11px 24px', borderRadius: 8, border: 'none', background: C.brand, color: '#fff', fontSize: 14, fontWeight: 600, cursor: 'pointer', fontFamily: "'DM Sans', sans-serif", boxShadow: '0 2px 8px rgba(217,119,87,0.3)' }}>
              ↻ Sync Now
            </button>
          </div>
        )}

        {/* ── Dashboard content (only when data loaded) ── */}
        {grand && !isLoading && (<>

        {/* ── OVERVIEW ── */}
        {view === 'overview' && !selectedSupplier && (<div>
          <div style={{ display: 'grid', gridTemplateColumns: 'repeat(4, 1fr)', gap: 14, marginBottom: 20 }}>
            {[
              { label: 'Total AP Balance',    value: fmt(grand.total),    sub: `${meta.totalSuppliers} suppliers · ${meta.totalInvoices} invoices`,      accent: C.brand },
              { label: 'Current (Not Due)',   value: fmt(grand.current),  sub: `${((grand.current/grand.total)*100).toFixed(1)}% of total`,              accent: '#5C8A6E' },
              { label: 'Total Overdue',       value: fmt(overdueTotal),   sub: `${overduePercent}% of total`,                                            accent: '#C8935A' },
              { label: 'Critical (61+ Days)', value: fmt(criticalTotal),  sub: `${((criticalTotal/grand.total)*100).toFixed(1)}% — escalate`,            accent: '#C05A3A' },
            ].map((kpi, i) => (
              <div key={i} style={{ ...card, position: 'relative', overflow: 'hidden' }}>
                <div style={{ position: 'absolute', top: 0, left: 0, right: 0, height: 3, background: kpi.accent, borderRadius: '12px 12px 0 0' }} />
                <p style={{ margin: '4px 0 6px', fontSize: 10.5, color: C.textMute, fontWeight: 600, textTransform: 'uppercase', letterSpacing: '0.07em' }}>{kpi.label}</p>
                <p style={{ margin: '0 0 4px', fontSize: 25, fontWeight: 700, fontFamily: "'JetBrains Mono', monospace", color: C.textDark }}>{kpi.value}</p>
                <p style={{ margin: 0, fontSize: 11, color: C.textMute }}>{kpi.sub}</p>
              </div>
            ))}
          </div>

          <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: 14, marginBottom: 14 }}>
            <div style={card}>
              <h3 style={{ margin: '0 0 14px', fontSize: 13.5, fontWeight: 600, color: C.textDark }}>Aging Breakdown</h3>
              <div style={{ display: 'flex', alignItems: 'center' }}>
                <ResponsiveContainer width="52%" height={196}>
                  <PieChart>
                    <Pie data={agingPieData} cx="50%" cy="50%" innerRadius={52} outerRadius={82} paddingAngle={2} dataKey="value" stroke="none">
                      {agingPieData.map((e, i) => <Cell key={i} fill={e.color} />)}
                    </Pie>
                    <Tooltip formatter={fmtFull} contentStyle={{ background: C.surface, border: `1px solid ${C.border}`, borderRadius: 8, fontSize: 12 }} />
                  </PieChart>
                </ResponsiveContainer>
                <div style={{ flex: 1 }}>
                  {AGING_KEYS.map(a => (
                    <div key={a.key} style={{ display: 'flex', alignItems: 'center', justifyContent: 'space-between', padding: '5px 0', borderBottom: `1px solid ${C.border}` }}>
                      <div style={{ display: 'flex', alignItems: 'center', gap: 8 }}>
                        <div style={{ width: 10, height: 10, borderRadius: 3, background: a.color }} />
                        <span style={{ fontSize: 12, color: C.textMid }}>{a.label}</span>
                      </div>
                      <div style={{ textAlign: 'right' }}>
                        <span style={{ fontSize: 12, fontWeight: 600, fontFamily: "'JetBrains Mono', monospace", color: C.textDark }}>{fmt(grand[a.key])}</span>
                        <span style={{ fontSize: 10, color: C.textMute, marginLeft: 6 }}>{((grand[a.key]/grand.total)*100).toFixed(1)}%</span>
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            </div>
            <div style={card}>
              <h3 style={{ margin: '0 0 14px', fontSize: 13.5, fontWeight: 600, color: C.textDark }}>Top 8 Suppliers by Balance</h3>
              <ResponsiveContainer width="100%" height={196}>
                <BarChart data={top8Bar} layout="vertical" margin={{ left: 10, right: 10, top: 0, bottom: 0 }}>
                  <XAxis type="number" tickFormatter={fmt} tick={{ fontSize: 10, fill: C.textMute }} axisLine={false} tickLine={false} />
                  <YAxis type="category" dataKey="name" width={130} tick={{ fontSize: 9, fill: C.textMid }} axisLine={false} tickLine={false} tickFormatter={(v) => v.length > 18 ? v.substring(0,18)+'…' : v} />
                  <Tooltip content={<CustomTooltipBar />} />
                  {AGING_KEYS.map((a, i) => (
                    <Bar key={a.key} dataKey={a.key} stackId="a" fill={a.color} name={a.label} radius={i === AGING_KEYS.length-1 ? [0,4,4,0] : [0,0,0,0]} />
                  ))}
                </BarChart>
              </ResponsiveContainer>
            </div>
          </div>

          <div style={{ display: 'grid', gridTemplateColumns: '2fr 1fr', gap: 14 }}>
            <div style={card}>
              <h3 style={{ margin: '0 0 2px', fontSize: 13.5, fontWeight: 600, color: C.textDark }}>Supplier Concentration</h3>
              <p style={{ margin: '0 0 12px', fontSize: 11, color: C.textMute }}>Size = balance · Color = urgency level</p>
              <ResponsiveContainer width="100%" height={224}>
                <Treemap data={treemapData} dataKey="size" nameKey="name" stroke={C.bg} strokeWidth={3}
                  content={({ x, y, width, height, name, color }) => {
                    const tooSmall = width < 48 || height < 28;
                    const maxChars = Math.floor(width / 7.5);
                    const label = name && name.length > maxChars ? name.substring(0, Math.max(maxChars - 1, 4)) + '…' : name;
                    return (
                      <g>
                        <rect x={x} y={y} width={width} height={height} fill={color} rx={4} />
                        {!tooSmall && (
                          <text x={x + width/2} y={y + height/2} textAnchor="middle" dominantBaseline="central"
                            fill="#1A1714" fontSize={Math.min(Math.max(Math.floor(width / 11), 9), 12)}
                            fontWeight="300" fontFamily="ui-sans-serif, system-ui, -apple-system, sans-serif"
                            stroke="none" style={{ pointerEvents: 'none' }}
                          >{label}</text>
                        )}
                      </g>
                    );
                  }} />
              </ResponsiveContainer>
            </div>
            <div style={card}>
              <h3 style={{ margin: '0 0 14px', fontSize: 13.5, fontWeight: 600, color: C.textDark }}>Urgency Distribution</h3>
              {['CRITICAL', 'ATTENTION', 'OK'].map(level => {
                const sups  = suppliers.filter(s => getUrgency(s) === level);
                const total = sups.reduce((acc, s) => acc + s.total, 0);
                const ri    = URGENCY_INFO[level];
                return (
                  <div key={level} style={{ marginBottom: 12, padding: 14, background: ri.bg, borderRadius: 9, border: `1px solid ${ri.border}` }}>
                    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: 4 }}>
                      <UrgencyBadge level={level} />
                      <span style={{ fontSize: 11, color: C.textMute }}>{sups.length} suppliers</span>
                    </div>
                    <p style={{ margin: '4px 0 5px', fontSize: 19, fontWeight: 700, fontFamily: "'JetBrains Mono', monospace", color: ri.text }}>{fmt(total)}</p>
                    <p style={{ margin: 0, fontSize: 11, color: ri.text, lineHeight: 1.45, opacity: 0.85 }}>{ri.desc}</p>
                  </div>
                );
              })}
            </div>
          </div>
        </div>)}

        {/* ── SUPPLIERS ── */}
        {view === 'suppliers' && !selectedSupplier && (<div>
          <div style={{ display: 'flex', gap: 10, marginBottom: 18, alignItems: 'center', flexWrap: 'wrap' }}>
            <input placeholder="Search suppliers..." value={searchTerm} onChange={(e) => setSearchTerm(e.target.value)}
              style={{ flex: 1, minWidth: 200, padding: '10px 16px', background: C.surface, border: `1px solid ${C.border}`, borderRadius: 9, color: C.textDark, fontSize: 13, outline: 'none', fontFamily: "'DM Sans', sans-serif" }} />
            <div style={{ display: 'flex', gap: 5 }}>
              {['ALL','CRITICAL','ATTENTION','OK'].map(u => (
                <button key={u} onClick={() => setFilterUrgency(u)} style={{
                  padding: '8px 13px', borderRadius: 7, border: '1px solid', cursor: 'pointer', fontSize: 11, fontWeight: 600, fontFamily: "'DM Sans', sans-serif",
                  background: filterUrgency === u ? (u === 'CRITICAL' ? '#C05A3A' : u === 'ATTENTION' ? '#C8935A' : u === 'OK' ? '#5C8A6E' : C.brand) : C.surface,
                  color: filterUrgency === u ? '#fff' : C.textMid,
                  borderColor: filterUrgency === u ? 'transparent' : C.border,
                }}>{u}</button>
              ))}
            </div>
            <select value={sortBy} onChange={(e) => setSortBy(e.target.value)}
              style={{ padding: '8px 13px', background: C.surface, border: `1px solid ${C.border}`, borderRadius: 7, color: C.textDark, fontSize: 12, fontFamily: "'DM Sans', sans-serif" }}>
              <option value="total">Sort: Balance</option>
              <option value="overdue">Sort: Overdue</option>
              <option value="urgency">Sort: Urgency</option>
              <option value="invoices">Sort: # Entries</option>
            </select>
          </div>
          <div style={{ display: 'grid', gap: 7 }}>
            {sortedSuppliers.map((s) => {
              const overdue = s.d30+s.d60+s.d90+s.d120+s.over120;
              const ui = URGENCY_INFO[getUrgency(s)];
              return (
                <div key={s.bp_code} onClick={() => setSelectedSupplier(s.bp_code)}
                  style={{ ...card, padding: '15px 20px', cursor: 'pointer', transition: 'border-color 0.15s, box-shadow 0.15s' }}
                  onMouseEnter={(e) => { e.currentTarget.style.borderColor = '#D9A890'; e.currentTarget.style.boxShadow = '0 2px 10px rgba(217,119,87,0.10)'; }}
                  onMouseLeave={(e) => { e.currentTarget.style.borderColor = C.border; e.currentTarget.style.boxShadow = '0 1px 3px rgba(90,60,40,0.05)'; }}>
                  <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: 10 }}>
                    <div style={{ display: 'flex', alignItems: 'center', gap: 11 }}>
                      <div style={{ width: 34, height: 34, borderRadius: 8, background: ui.bg, border: `1px solid ${ui.border}`, display: 'flex', alignItems: 'center', justifyContent: 'center', fontSize: 13, fontWeight: 700, color: ui.text }}>
                        {s.name.charAt(0)}
                      </div>
                      <div>
                        <p style={{ margin: 0, fontSize: 13, fontWeight: 600, color: C.textDark }}>{s.name}</p>
                        <p style={{ margin: 0, fontSize: 11, color: C.textMute }}>{s.bp_code} · {s.invoices} entries</p>
                      </div>
                    </div>
                    <div style={{ display: 'flex', alignItems: 'center', gap: 14 }}>
                      <UrgencyBadge level={getUrgency(s)} />
                      <div style={{ textAlign: 'right' }}>
                        <p style={{ margin: 0, fontSize: 16, fontWeight: 700, fontFamily: "'JetBrains Mono', monospace", color: C.textDark }}>{fmt(s.total)}</p>
                        {overdue > 0 && <p style={{ margin: 0, fontSize: 11, color: '#C8935A' }}>Overdue: {fmt(overdue)}</p>}
                      </div>
                    </div>
                  </div>
                  <div style={{ display: 'flex', height: 5, borderRadius: 3, overflow: 'hidden', background: '#F0EBE5' }}>
                    {AGING_KEYS.map(a => { const pct = (s[a.key]/s.total)*100; return pct > 0 ? <div key={a.key} style={{ width: `${pct}%`, background: a.color }} title={`${a.label}: ${fmtFull(s[a.key])}`} /> : null; })}
                  </div>
                </div>
              );
            })}
          </div>
        </div>)}

        {/* ── SUPPLIER DETAIL ── */}
        {selectedSupplier && selectedData && (<div>
          <button onClick={() => setSelectedSupplier(null)} style={{ background: 'none', border: 'none', color: C.brand, cursor: 'pointer', fontSize: 13, marginBottom: 16, padding: 0, fontFamily: "'DM Sans', sans-serif", fontWeight: 600 }}>← Back</button>
          <div style={{ display: 'flex', alignItems: 'center', gap: 14, marginBottom: 18 }}>
            <h2 style={{ margin: 0, fontSize: 21, fontWeight: 700, color: C.textDark }}>{selectedData.name}</h2>
            <UrgencyBadge level={getUrgency(selectedData)} />
          </div>
          <div style={{ display: 'grid', gridTemplateColumns: 'repeat(3, 1fr)', gap: 12, marginBottom: 18 }}>
            {[
              { label: 'Total Payable',     value: fmtFull(selectedData.total) },
              { label: 'Current (Not Due)', value: fmtFull(selectedData.current) },
              { label: 'Total Overdue',     value: fmtFull(selectedData.d30+selectedData.d60+selectedData.d90+selectedData.d120+selectedData.over120) },
            ].map((kpi, i) => (
              <div key={i} style={{ ...card, padding: '14px 18px' }}>
                <p style={{ margin: '0 0 4px', fontSize: 10.5, color: C.textMute, textTransform: 'uppercase', letterSpacing: '0.06em' }}>{kpi.label}</p>
                <p style={{ margin: 0, fontSize: 18, fontWeight: 700, fontFamily: "'JetBrains Mono', monospace", color: C.textDark }}>{kpi.value}</p>
              </div>
            ))}
          </div>
          <div style={{ ...card, marginBottom: 18 }}>
            <h4 style={{ margin: '0 0 12px', fontSize: 13, fontWeight: 600, color: C.textDark }}>Aging Distribution</h4>
            <div style={{ display: 'flex', height: 26, borderRadius: 5, overflow: 'hidden' }}>
              {AGING_KEYS.map(a => { const pct = (selectedData[a.key]/selectedData.total)*100; return pct > 0 ? (
                <div key={a.key} style={{ width: `${pct}%`, background: a.color, display: 'flex', alignItems: 'center', justifyContent: 'center', fontSize: 10, fontWeight: 700, color: '#fff' }}>
                  {pct > 8 ? `${a.label} ${pct.toFixed(0)}%` : ''}</div>) : null; })}
            </div>
            <div style={{ display: 'flex', gap: 16, marginTop: 10, flexWrap: 'wrap' }}>
              {AGING_KEYS.filter(a => selectedData[a.key] > 0).map(a => (
                <div key={a.key} style={{ display: 'flex', alignItems: 'center', gap: 6 }}>
                  <div style={{ width: 8, height: 8, borderRadius: 2, background: a.color }} />
                  <span style={{ fontSize: 11, color: C.textMid }}>{a.label}:</span>
                  <span style={{ fontSize: 11, fontWeight: 600, fontFamily: "'JetBrains Mono', monospace", color: C.textDark }}>{fmtFull(selectedData[a.key])}</span>
                </div>
              ))}
            </div>
          </div>

          {/* Per-document breakdown */}
          {(() => {
            const docs = rawDocs.filter(d => d.bp === selectedSupplier).sort((a,b) => b.dv - a.dv);
            const getBucket = (dv) => {
              if (dv <= 0)   return { label: 'Current', color: AGING_KEYS[0].color, action: 'Schedule for due date',   actionColor: '#5C8A6E' };
              if (dv <= 30)  return { label: '1–30',    color: AGING_KEYS[1].color, action: 'Prepare payment run',    actionColor: '#C8935A' };
              if (dv <= 60)  return { label: '31–60',   color: AGING_KEYS[2].color, action: 'Urgent — queue payment', actionColor: '#D97757' };
              if (dv <= 90)  return { label: '61–90',   color: AGING_KEYS[3].color, action: 'ESCALATE immediately',   actionColor: '#C05A3A' };
              if (dv <= 120) return { label: '91–120',  color: AGING_KEYS[4].color, action: 'ESCALATE immediately',   actionColor: '#C05A3A' };
              return           { label: '120+',   color: AGING_KEYS[5].color, action: 'ESCALATE immediately',   actionColor: '#C05A3A' };
            };
            const hasDetail = docs.length > 0;
            return (
              <div style={{ ...card, padding: 0, overflow: 'hidden' }}>
                <div style={{ padding: '13px 20px', borderBottom: `1px solid ${C.border}`, display: 'flex', alignItems: 'center', justifyContent: 'space-between' }}>
                  <h4 style={{ margin: 0, fontSize: 13, fontWeight: 600, color: C.textDark }}>
                    Open Entries ({hasDetail ? docs.length : selectedData.invoices} {hasDetail ? 'documents' : 'total'})
                  </h4>
                  <span style={{ fontSize: 11, color: C.textMute }}>{hasDetail ? 'Per-document · sorted by days overdue' : 'Document detail not in source data for this supplier'}</span>
                </div>
                <div style={{ maxHeight: 420, overflowY: 'auto' }}>
                  <table style={{ width: '100%', borderCollapse: 'collapse', fontSize: 12 }}>
                    <thead style={{ position: 'sticky', top: 0, zIndex: 1 }}>
                      <tr style={{ background: C.tableStripe }}>
                        {['Document No.','Supplier Invoice','Doc Date','Due Date','Days OVD','Currency','Balance (PHP)','Aging','Action'].map(h => (
                          <th key={h} style={{ padding: '9px 13px', textAlign: ['Balance (PHP)','Days OVD'].includes(h) ? 'right' : 'left', color: C.textMid, fontWeight: 600, fontSize: 10.5, borderBottom: `1px solid ${C.border}`, whiteSpace: 'nowrap' }}>{h}</th>
                        ))}
                      </tr>
                    </thead>
                    <tbody>
                      {hasDetail ? docs.map((d, i) => {
                        const bkt = getBucket(d.dv);
                        return (
                          <tr key={i} style={{ borderBottom: `1px solid ${C.border}`, background: i % 2 === 0 ? C.surface : C.tableStripe }}>
                            <td style={{ padding: '8px 13px', fontFamily: "'JetBrains Mono', monospace", fontSize: 11, color: C.textDark, whiteSpace: 'nowrap' }}>{d.dn}</td>
                            <td style={{ padding: '8px 13px', fontSize: 11, color: C.textMid, maxWidth: 160, overflow: 'hidden', textOverflow: 'ellipsis', whiteSpace: 'nowrap' }}>{d.si || '—'}</td>
                            <td style={{ padding: '8px 13px', fontSize: 11, color: C.textMid, whiteSpace: 'nowrap' }}>{d.dd || '—'}</td>
                            <td style={{ padding: '8px 13px', fontSize: 11, color: d.dv > 0 ? '#C05A3A' : C.textMid, whiteSpace: 'nowrap' }}>{d.du || '—'}</td>
                            <td style={{ padding: '8px 13px', textAlign: 'right', fontFamily: "'JetBrains Mono', monospace", fontSize: 11, fontWeight: 600, color: d.dv > 60 ? '#C05A3A' : d.dv > 0 ? '#C8935A' : '#5C8A6E' }}>
                              {d.dv > 0 ? `+${d.dv}` : d.dv === 0 ? '0' : d.dv}
                            </td>
                            <td style={{ padding: '8px 13px', fontSize: 11, color: C.textMid }}>{d.cu}</td>
                            <td style={{ padding: '8px 13px', textAlign: 'right', fontFamily: "'JetBrains Mono', monospace", fontSize: 11, fontWeight: 600, color: C.textDark }}>{fmtFull(d.b)}</td>
                            <td style={{ padding: '8px 13px' }}>
                              <span style={{ display: 'inline-flex', alignItems: 'center', gap: 5, fontSize: 10.5, fontWeight: 600, color: C.textDark }}>
                                <span style={{ width: 8, height: 8, borderRadius: 2, background: bkt.color, display: 'inline-block', flexShrink: 0 }} />{bkt.label}
                              </span>
                            </td>
                            <td style={{ padding: '8px 13px', fontSize: 10.5, fontWeight: 600, color: bkt.actionColor, whiteSpace: 'nowrap' }}>{bkt.action}</td>
                          </tr>
                        );
                      }) : (
                        <tr><td colSpan={9} style={{ padding: '24px', textAlign: 'center', color: C.textMute, fontSize: 12 }}>
                          Document-level detail not included in the source data for this supplier.
                        </td></tr>
                      )}
                    </tbody>
                  </table>
                </div>
              </div>
            );
          })()}
        </div>)}

        {/* ── PAYMENT QUEUE ── */}
        {view === 'payments' && !selectedSupplier && (<div>
          <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: 14, marginBottom: 18 }}>
            <div style={card}>
              <h3 style={{ margin: '0 0 3px', fontSize: 13.5, fontWeight: 600, color: C.textDark }}>Critical Payment Queue</h3>
              <p style={{ margin: '0 0 14px', fontSize: 11, color: C.textMute }}>61+ day overdue — ranked by critical amount</p>
              <div style={{ maxHeight: 340, overflowY: 'auto' }}>
                {suppliers.filter(s => (s.d90+s.d120+s.over120) > 0)
                  .sort((a,b) => (b.d90+b.d120+b.over120)-(a.d90+a.d120+a.over120))
                  .map((s, i) => {
                    const critical = s.d90+s.d120+s.over120;
                    return (
                      <div key={i} style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', padding: '10px', borderBottom: `1px solid ${C.border}`, cursor: 'pointer', borderRadius: 6 }}
                        onClick={() => setSelectedSupplier(s.bp_code)}
                        onMouseEnter={(e) => e.currentTarget.style.background = C.tableStripe}
                        onMouseLeave={(e) => e.currentTarget.style.background = 'transparent'}>
                        <div style={{ flex: 1 }}>
                          <p style={{ margin: 0, fontSize: 12, fontWeight: 600, color: C.textDark }}>{s.name}</p>
                          <p style={{ margin: 0, fontSize: 10, color: C.textMute }}>{s.invoices} open entries · {s.bp_code}</p>
                        </div>
                        <div style={{ textAlign: 'right', marginLeft: 12 }}>
                          <p style={{ margin: 0, fontSize: 13, fontWeight: 700, fontFamily: "'JetBrains Mono', monospace", color: '#C05A3A' }}>{fmtFull(critical)}</p>
                          <p style={{ margin: 0, fontSize: 10, color: '#C05A3A', fontWeight: 600 }}>61+ days overdue</p>
                        </div>
                      </div>
                    );
                  })}
              </div>
            </div>
            <div style={card}>
              <h3 style={{ margin: '0 0 3px', fontSize: 13.5, fontWeight: 600, color: C.textDark }}>Upcoming Payments (Current)</h3>
              <p style={{ margin: '0 0 14px', fontSize: 11, color: C.textMute }}>Not yet due — prepare for upcoming maturities</p>
              <div style={{ maxHeight: 340, overflowY: 'auto' }}>
                {suppliers.filter(s => s.current > 0).sort((a,b) => b.current - a.current).map((s, i) => (
                  <div key={i} style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', padding: '10px', borderBottom: `1px solid ${C.border}`, cursor: 'pointer', borderRadius: 6 }}
                    onClick={() => setSelectedSupplier(s.bp_code)}
                    onMouseEnter={(e) => e.currentTarget.style.background = C.tableStripe}
                    onMouseLeave={(e) => e.currentTarget.style.background = 'transparent'}>
                    <div style={{ flex: 1 }}>
                      <p style={{ margin: 0, fontSize: 12, fontWeight: 600, color: C.textDark }}>{s.name}</p>
                      <p style={{ margin: 0, fontSize: 10, color: C.textMute }}>Current · not yet due</p>
                    </div>
                    <p style={{ margin: 0, fontSize: 13, fontWeight: 700, fontFamily: "'JetBrains Mono', monospace", color: '#5C8A6E' }}>{fmtFull(s.current)}</p>
                  </div>
                ))}
              </div>
            </div>
          </div>
          <div style={card}>
            <h3 style={{ margin: '0 0 3px', fontSize: 13.5, fontWeight: 600, color: C.textDark }}>Full Payable Aging Summary</h3>
            <p style={{ margin: '0 0 14px', fontSize: 11, color: C.textMute }}>All suppliers with outstanding balances — click to drill down</p>
            <div style={{ overflowX: 'auto' }}>
              <table style={{ width: '100%', borderCollapse: 'collapse', fontSize: 12 }}>
                <thead><tr style={{ background: C.tableStripe }}>
                  {['Supplier','Status','Total Balance','1–30 Days','31–60 Days','61–90 Days','91–120 Days','120+ Days','Total Overdue'].map(h => (
                    <th key={h} style={{ padding: '9px 13px', textAlign: h === 'Supplier' ? 'left' : 'right', color: C.textMid, fontWeight: 600, fontSize: 11, borderBottom: `1px solid ${C.border}`, whiteSpace: 'nowrap' }}>{h}</th>
                  ))}
                </tr></thead>
                <tbody>
                  {suppliers.filter(s => (s.d30+s.d60+s.d90+s.d120+s.over120) > 0)
                    .sort((a,b) => (b.d30+b.d60+b.d90+b.d120+b.over120)-(a.d30+a.d60+a.d90+a.d120+a.over120))
                    .map((s, i) => {
                      const overdue = s.d30+s.d60+s.d90+s.d120+s.over120;
                      return (
                        <tr key={i} onClick={() => setSelectedSupplier(s.bp_code)} style={{ borderBottom: `1px solid ${C.border}`, cursor: 'pointer' }}
                          onMouseEnter={(e) => e.currentTarget.style.background = C.tableStripe}
                          onMouseLeave={(e) => e.currentTarget.style.background = 'transparent'}>
                          <td style={{ padding: '9px 13px', fontWeight: 500, color: C.textDark }}>{s.name}</td>
                          <td style={{ padding: '9px 13px', textAlign: 'right' }}><UrgencyBadge level={getUrgency(s)} /></td>
                          <td style={{ padding: '9px 13px', textAlign: 'right', fontFamily: "'JetBrains Mono', monospace", color: C.textDark }}>{fmt(s.total)}</td>
                          {['d30','d60','d90','d120','over120'].map(k => (
                            <td key={k} style={{ padding: '9px 13px', textAlign: 'right', fontFamily: "'JetBrains Mono', monospace", color: s[k] > 0 ? AGING_KEYS.find(a=>a.key===k).color : C.border }}>
                              {s[k] > 0 ? fmt(s[k]) : '—'}</td>
                          ))}
                          <td style={{ padding: '9px 13px', textAlign: 'right', fontWeight: 700, fontFamily: "'JetBrains Mono', monospace", color: '#C8935A' }}>{fmt(overdue)}</td>
                        </tr>
                      );
                    })}
                </tbody>
              </table>
            </div>
          </div>
        </div>)}

        </>)}
      </div>
    </div>
  );
}
