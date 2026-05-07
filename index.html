// ============================================================
// MIRASCOPE MATH VERIFICATION
// Ground truth from Adhya & Noé (Stony Brook, 2007)
// Measured Mirage Model 2000: f = 8.06 cm, D = 22.8 cm, A = 6.25 cm
// Run with: node verify.js
// ============================================================

const MM = 1;        // working in millimeters throughout
const DEG = 180 / Math.PI;

// ─── Known values from the paper ────────────────────────────
const KNOWN = {
  f:     80.6,   // mm  focal length (measured)
  D:     228,    // mm  mirror diameter (measured)
  A:     62.5,   // mm  aperture diameter (measured)
  dSep:  80.6,   // mm  vertex separation (measured = 1 × f)
};

// ─── Formula 1: dish depth ───────────────────────────────────
// d = D² / (16f)
// Paper doesn't state d directly but we can verify:
// d should equal dSep/2 for canonical mirascope (rims meet)
function dishDepth(D, f) {
  return (D * D) / (16 * f);
}

// ─── Formula 2: image height above aperture ─────────────────
// From matrix optics (Adhya & Noé Fig 3):
// When object is lifted h_obj above M1 vertex:
//   h_img = f * h_obj / (f - h_obj)
// At h_obj = 0: image is AT the aperture (h_img = 0) ✓
function imageHeight(f, h_obj) {
  if (Math.abs(f - h_obj) < 1e-9) return Infinity; // image at infinity
  return (f * h_obj) / (f - h_obj);
}

// ─── Formula 3: magnification ────────────────────────────────
// mag = f / (f - h_obj)
// At h_obj = 0: mag = 1.0 (unit magnification) ✓
// Fig 3 of paper: at h_obj = 1 inch = 25.4mm, mag ≈ 1.45
function magnification(f, h_obj) {
  if (Math.abs(f - h_obj) < 1e-9) return Infinity;
  return f / (f - h_obj);
}

// ─── Formula 4: image x position (inversion) ─────────────────
// image_x = -object_x * mag
// Red LED at x=-20 → image at x=+20*mag (right side) ✓
function imageX(obj_x, f, h_obj) {
  return -obj_x * magnification(f, h_obj);
}

// ─── Formula 5: backward solve — object height from desired image height ──
// h_obj = f * h_img / (f + h_img)
// This is the key equation for the drag-the-image sim
function solveObjectHeight(f, desired_h_img) {
  return (f * desired_h_img) / (f + desired_h_img);
}

// ─── Formula 6: viewing cone ─────────────────────────────────
// θ_min = arctan(A/2 / dSep)  shallowest angle from horizontal
// θ_max = arctan(D/2 / dSep)  steepest angle from horizontal
// UAF physics demo empirical: optimal ≈ 45°, works 30°–60°
function viewingCone(D, A, dSep) {
  const theta_min = Math.atan((A / 2) / dSep) * DEG;
  const theta_max = Math.atan((D / 2) / dSep) * DEG;
  return {
    theta_min: theta_min,
    theta_max: theta_max,
    width: theta_max - theta_min,
  };
}

// ─── Formula 7: image exists check ───────────────────────────
// Canonical: f1 = f2 = dSep → perfect image, zero aberration
// Tolerance: 1% of f
function imageExists(f1, f2, dSep) {
  const tol = f1 * 0.01;
  const sym = Math.abs(f1 - f2) < tol;
  const sep = Math.abs(f1 - dSep) < tol;
  if (sym && sep) return { exists: true, quality: 'perfect', aberration: 0 };
  if (sym && !sep) {
    // image still forms but not at aperture, aberration grows
    const ratio = dSep / f1;
    if (ratio >= 2.0) return { exists: false, reason: 'dSep >= 2f, rays parallel' };
    if (ratio <= 0)   return { exists: false, reason: 'dSep <= 0' };
    return { exists: true, quality: 'degraded', aberration: Math.abs(dSep - f1) };
  }
  return { exists: true, quality: 'asymmetric', aberration: Math.abs(f1 - f2) };
}

// ─── Formula 8: canonical geometry relationship ───────────────
// For rims to meet: 2d = dSep → d = dSep/2
// Combined with d = D²/16f and dSep = f:
//   f = D / (2√2) ≈ 0.3536 × D
function canonicalF(D) {
  return D / (2 * Math.sqrt(2));
}

// ─── Formula 9: surface normal at hit point ───────────────────
// nx = -(x - vx) / (2f)
// ny = +1 opens up (M1), -1 opens down (M2)
// normalized: divide by sqrt(nx²+ny²)
function surfaceNormal(x, vx, f, opensUp) {
  const nx = -(x - vx) / (2 * f);
  const ny = opensUp ? 1 : -1;
  const len = Math.hypot(nx, ny);
  return { x: nx / len, y: ny / len };
}

// ─── Formula 10: reflection ───────────────────────────────────
// r = d - 2(d·n)n
function reflectDir(d, n) {
  const dot = d.x * n.x + d.y * n.y;
  return { x: d.x - 2 * dot * n.x, y: d.y - 2 * dot * n.y };
}

// ─── VERIFICATION RUNS ───────────────────────────────────────

function line() { console.log('─'.repeat(60)); }
function header(s) { line(); console.log(s); line(); }

header('TEST 1: Dish depth — Mirage Model 2000');
{
  const d = dishDepth(KNOWN.D, KNOWN.f);
  const dSepHalf = KNOWN.dSep / 2;
  console.log(`  D = ${KNOWN.D}mm, f = ${KNOWN.f}mm`);
  console.log(`  d = D²/16f = ${d.toFixed(2)} mm`);
  console.log(`  dSep/2     = ${dSepHalf.toFixed(2)} mm`);
  console.log(`  Match (d ≈ dSep/2)? ${Math.abs(d - dSepHalf) < 0.1 ? '✓ YES' : '✗ NO'}`);
  console.log(`  Expected: ~40.3 mm`);
}

header('TEST 2: Image height — object at vertex (h_obj = 0)');
{
  const h_img = imageHeight(KNOWN.f, 0);
  const mag   = magnification(KNOWN.f, 0);
  console.log(`  h_obj = 0mm → h_img = ${h_img.toFixed(2)} mm`);
  console.log(`  Magnification = ${mag.toFixed(4)}`);
  console.log(`  Expected: h_img = 0, mag = 1.0000 ✓`);
}

header('TEST 3: Image height — object lifted (reproduce Fig 3)');
{
  // Paper Fig 3: at h_obj = 1 inch = 25.4mm, mag ≈ 1.45
  const h_obj_vals = [0, 5, 10, 15, 20, 25.4];
  console.log(`  ${'h_obj(mm)'.padEnd(12)} ${'h_img(mm)'.padEnd(12)} ${'mag'.padEnd(10)} ${'img_disp/h_obj'}`);
  h_obj_vals.forEach(h => {
    const h_img = imageHeight(KNOWN.f, h);
    const mag   = magnification(KNOWN.f, h);
    const ratio = h > 0 ? (h_img / h).toFixed(3) : '—';
    console.log(`  ${h.toFixed(1).padEnd(12)} ${h_img.toFixed(2).padEnd(12)} ${mag.toFixed(4).padEnd(10)} ${ratio}`);
  });
  console.log(`  Paper says: at h_obj=25.4mm, mag ≈ 1.45`);
  console.log(`  Our formula: mag = ${magnification(KNOWN.f, 25.4).toFixed(4)}`);
}

header('TEST 4: Backward solve — find object height for desired image height');
{
  const desired_heights = [0, 10, 20, 40, 80];
  console.log(`  ${'desired h_img(mm)'.padEnd(20)} ${'h_obj needed(mm)'.padEnd(20)} ${'verify: h_img back'}`);
  desired_heights.forEach(h_img => {
    const h_obj = solveObjectHeight(KNOWN.f, h_img);
    const verify = imageHeight(KNOWN.f, h_obj);
    console.log(`  ${h_img.toFixed(1).padEnd(20)} ${h_obj.toFixed(3).padEnd(20)} ${verify.toFixed(3)}`);
  });
  console.log(`  Each verify column should match desired h_img ✓`);
}

header('TEST 5: Viewing cone — Mirage Model 2000');
{
  const cone = viewingCone(KNOWN.D, KNOWN.A, KNOWN.dSep);
  console.log(`  D=${KNOWN.D}mm, A=${KNOWN.A}mm, dSep=${KNOWN.dSep}mm`);
  console.log(`  θ_min (shallowest) = ${cone.theta_min.toFixed(1)}°`);
  console.log(`  θ_max (steepest)   = ${cone.theta_max.toFixed(1)}°`);
  console.log(`  Cone width         = ${cone.width.toFixed(1)}°`);
  console.log(`  UAF empirical: optimal ≈ 45°, works 30–60°`);
  console.log(`  Our midpoint: ${((cone.theta_min + cone.theta_max)/2).toFixed(1)}°`);
}

header('TEST 6: Image exists check');
{
  const cases = [
    { f1: 80.6, f2: 80.6, dSep: 80.6, label: 'Canonical (perfect)' },
    { f1: 80.6, f2: 80.6, dSep: 100,  label: 'dSep > f (image moves)' },
    { f1: 80.6, f2: 80.6, dSep: 161.2,label: 'dSep = 2f (no image)' },
    { f1: 80,   f2: 120,  dSep: 100,  label: 'Asymmetric f1≠f2' },
  ];
  cases.forEach(c => {
    const r = imageExists(c.f1, c.f2, c.dSep);
    console.log(`  ${c.label}`);
    console.log(`    → exists=${r.exists}, quality=${r.quality || r.reason}`);
  });
}

header('TEST 7: Canonical geometry — does f = D/(2√2)?');
{
  const Ds = [228, 280, 300, 400];
  console.log(`  ${'D(mm)'.padEnd(10)} ${'f=D/2√2'.padEnd(12)} ${'d=f/2'.padEnd(12)} ${'verify 2d=f?'}`);
  Ds.forEach(D => {
    const f = canonicalF(D);
    const d = dishDepth(D, f);
    const check = Math.abs(2*d - f) < 0.01;
    console.log(`  ${D.toString().padEnd(10)} ${f.toFixed(2).padEnd(12)} ${d.toFixed(2).padEnd(12)} ${check ? '✓' : '✗'}`);
  });
}

header('TEST 8: Surface normal — verify parabola property');
{
  // For M2 (opensDown), object at its focal point (0, 0).
  // A ray from (0,0) hitting M2 at x = -40.54mm should reflect STRAIGHT DOWN
  // i.e. reflected direction should be (0, -1) exactly.
  const f = 80.6;
  const v2y = 80.6; // M2 vertex y
  const x_hit = -40.54;
  const y_hit = v2y - (x_hit * x_hit) / (4 * f); // on M2 surface

  // incoming direction: from object (0,0) to hit point
  const dx = x_hit - 0, dy = y_hit - 0;
  const len = Math.hypot(dx, dy);
  const dir = { x: dx/len, y: dy/len };

  const n = surfaceNormal(x_hit, 0, f, false); // M2 opens down
  const r = reflectDir(dir, n);

  console.log(`  Hit point on M2: (${x_hit}, ${y_hit.toFixed(2)})`);
  console.log(`  Incoming direction: (${dir.x.toFixed(4)}, ${dir.y.toFixed(4)})`);
  console.log(`  Surface normal:    (${n.x.toFixed(4)}, ${n.y.toFixed(4)})`);
  console.log(`  Reflected:         (${r.x.toFixed(4)}, ${r.y.toFixed(4)})`);
  console.log(`  Expected:          (0.0000, -1.0000) — straight down`);
  console.log(`  Correct? ${Math.abs(r.x) < 0.001 && Math.abs(r.y + 1) < 0.001 ? '✓ YES' : '✗ NO'}`);
}

header('TEST 9: Full round trip — object at vertex, image at aperture');
{
  // Object at (0, 0) = M1 vertex = M2 focal point
  // After M2: ray goes straight down (0, -1)
  // After M1: ray converges to M1 focal point = (0, f) = M2 vertex = aperture
  // So image_x should = 0, image_y should = f
  const f = 80.6;
  const v1y = -f/2; // M1 vertex
  const v2y = +f/2; // M2 vertex

  // Trace one ray: emit at theta = -0.5 rad from vertical
  const theta = -0.5;
  const dir = { x: Math.sin(theta), y: Math.cos(theta) };

  // Intersect with M2
  // y = v2y - x²/(4f), substitute ray P(t) = (t·sin, t·cos) from obj at (0, v1y)
  const ox = 0, oy = v1y;
  const dx = dir.x, dy = dir.y;
  const s = -1; // opensDown
  const u = ox, v = oy - v2y;
  const A = dx*dx;
  const B = 2*u*dx - 4*f*s*dy;
  const C = u*u - 4*f*s*v;
  const disc = B*B - 4*A*C;
  const t2 = (-B - Math.sqrt(disc)) / (2*A) > 1e-4
    ? (-B - Math.sqrt(disc)) / (2*A)
    : (-B + Math.sqrt(disc)) / (2*A);

  const hx2 = ox + dx*t2, hy2 = oy + dy*t2;

  // Reflect off M2
  const n2 = surfaceNormal(hx2, 0, f, false);
  const r1 = reflectDir(dir, n2);

  // Intersect with M1 — ray starts at (hx2, hy2) traveling in direction r1
  // M1: y = v1y + x²/(4f)  (opensUp, s=+1)
  // For r1 ≈ (0,-1) (vertical), use direct formula: t = hy2 - v1y - hx2²/(4f)
  // General case: solve quadratic
  let t1;
  const u1 = hx2, v1v = hy2 - v1y;
  const A1 = r1.x * r1.x;
  if (Math.abs(A1) < 1e-9) {
    // Ray is vertical — direct solution
    t1 = hy2 - v1y - (hx2 * hx2) / (4 * f);
  } else {
    const B1 = 2*u1*r1.x - 4*f*r1.y;
    const C1 = u1*u1 - 4*f*v1v;
    const disc1 = B1*B1 - 4*A1*C1;
    const t1a = (-B1 - Math.sqrt(disc1)) / (2*A1);
    const t1b = (-B1 + Math.sqrt(disc1)) / (2*A1);
    t1 = t1a > 1e-4 ? t1a : t1b;
  }

  const hx1 = hx2 + r1.x*t1, hy1 = hy2 + r1.y*t1;

  // Reflect off M1
  const n1 = surfaceNormal(hx1, 0, f, true);
  const r2 = reflectDir(r1, n1);

  // Where does exit ray cross aperture plane y = v2y?
  const t_img = (v2y - hy1) / r2.y;
  const img_x = hx1 + r2.x * t_img;
  const img_y = v2y;

  console.log(`  f = ${f}mm, dSep = ${f}mm`);
  console.log(`  Emission angle θ = ${theta} rad`);
  console.log(`  Hit M2 at:  (${hx2.toFixed(3)}, ${hy2.toFixed(3)})`);
  console.log(`  Reflected r1: (${r1.x.toFixed(4)}, ${r1.y.toFixed(4)}) — expect ≈ (0, -1)`);
  console.log(`  Hit M1 at:  (${hx1.toFixed(3)}, ${hy1.toFixed(3)})`);
  console.log(`  Exit dir r2: (${r2.x.toFixed(4)}, ${r2.y.toFixed(4)})`);
  console.log(`  Image at:   (${img_x.toFixed(3)}, ${img_y.toFixed(3)})`);
  console.log(`  Expected:   (0.000, ${v2y.toFixed(3)})`);
  console.log(`  Correct? ${Math.abs(img_x) < 0.1 && Math.abs(img_y - v2y) < 0.1 ? '✓ YES' : '✗ NO'}`);
}

header('SUMMARY — paste your output here and compare');
console.log(`
  Test 1:  d ≈ 40.3mm ✓
  Test 2:  h_img=0, mag=1.0 ✓
  Test 3:  at h_obj=25.4mm → mag ≈ 1.459 (paper says ≈1.45) ✓
  Test 4:  backward solve round-trips exactly ✓
  Test 5:  cone ≈ 21° to 55°, midpoint ≈ 38° (empirical 30-60°) ✓
  Test 6:  canonical=perfect, 2f=no image ✓
  Test 7:  2d = f for all D ✓
  Test 8:  reflected ray = (0, -1) straight down ✓
  Test 9:  image at (0, f/2+f/2) = (0, v2y) ✓

  If any show ✗, paste the full output and we fix that formula first.
`);
