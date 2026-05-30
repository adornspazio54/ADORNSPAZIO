# ADORNSPAZIO
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard — Select User</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #f5f4f0; min-height: 100vh; color: #1a1a1a; }

/* ── PASSWORD SCREEN ── */
#password-screen {
  min-height: 100vh; display: none; flex-direction: column; align-items: center; justify-content: center;
  background: linear-gradient(135deg, #f5f4f0 0%, #ede9e0 100%); padding: 40px 20px;
}
.pw-card {
  background: #fff; border: 2px solid #e8e6e0; border-radius: 24px; padding: 36px 32px;
  width: 100%; max-width: 360px; box-shadow: 0 8px 32px rgba(0,0,0,0.08); text-align: center;
}
.pw-avatar { font-size: 52px; margin-bottom: 10px; display: block; }
.pw-name { font-size: 20px; font-weight: 700; margin-bottom: 4px; }
.pw-sub { font-size: 13px; color: #999; margin-bottom: 24px; }
.pw-input-wrap { position: relative; margin-bottom: 14px; }
.pw-input { width: 100%; padding: 12px 44px 12px 14px; font-size: 15px; border: 1.5px solid #ddd; border-radius: 12px; font-family: inherit; background: #fafafa; color: #1a1a1a; letter-spacing: 2px; transition: border-color 0.15s; }
.pw-input:focus { outline: none; border-color: #1a1a1a; background: #fff; }
.pw-input.error { border-color: #ef4444; background: #fff5f5; }
.pw-eye { position: absolute; right: 12px; top: 50%; transform: translateY(-50%); background: none; border: none; cursor: pointer; font-size: 18px; color: #aaa; padding: 2px; }
.pw-eye:hover { color: #555; }
.pw-error { font-size: 12px; color: #ef4444; margin-bottom: 12px; min-height: 16px; }
.pw-submit { width: 100%; padding: 13px; font-size: 15px; font-weight: 700; border: none; border-radius: 12px; cursor: pointer; transition: all 0.15s; margin-bottom: 10px; }
.pw-submit.priyanka-btn { background: #db2777; color: #fff; }
.pw-submit.priyanka-btn:hover { background: #be185d; }
.pw-submit.pravin-btn { background: #2563eb; color: #fff; }
.pw-submit.pravin-btn:hover { background: #1d4ed8; }
.pw-submit.admin-btn { background: #d97706; color: #fff; }
.pw-submit.admin-btn:hover { background: #b45309; }
.pw-back { font-size: 13px; color: #aaa; background: none; border: none; cursor: pointer; padding: 4px 8px; border-radius: 8px; }
.pw-back:hover { color: #555; background: #f1f0eb; }
.pw-hint { font-size: 11px; color: #bbb; margin-top: 14px; }

/* ── CHANGE PASSWORD MODAL ── */
.cpw-modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.4); z-index: 3000; align-items: center; justify-content: center; padding: 20px; }
.cpw-modal-overlay.open { display: flex; }
.cpw-modal { background: #fff; border-radius: 20px; padding: 24px 22px; width: 100%; max-width: 380px; animation: slideup 0.22s ease; }
.cpw-modal h2 { font-size: 16px; font-weight: 700; margin-bottom: 18px; }
.cpw-group { display: flex; flex-direction: column; gap: 5px; margin-bottom: 14px; }
.cpw-group label { font-size: 11px; font-weight: 700; color: #666; text-transform: uppercase; letter-spacing: 0.5px; }
.cpw-input-wrap { position: relative; }
.cpw-input { width: 100%; padding: 10px 40px 10px 12px; font-size: 14px; border: 1.5px solid #ddd; border-radius: 10px; font-family: inherit; background: #fafafa; color: #1a1a1a; transition: border-color 0.15s; }
.cpw-input:focus { outline: none; border-color: #1a1a1a; background: #fff; }
.cpw-input.error { border-color: #ef4444; }
.cpw-eye { position: absolute; right: 10px; top: 50%; transform: translateY(-50%); background: none; border: none; cursor: pointer; font-size: 16px; color: #aaa; }
.cpw-error { font-size: 12px; color: #ef4444; margin-top: -10px; margin-bottom: 6px; min-height: 14px; }
.cpw-strength { height: 4px; border-radius: 4px; background: #f0ede8; margin-top: 6px; overflow: hidden; }
.cpw-strength-bar { height: 100%; border-radius: 4px; transition: width 0.3s, background 0.3s; width: 0%; }
.cpw-strength-label { font-size: 10px; color: #aaa; margin-top: 3px; }
.cpw-actions { display: flex; gap: 8px; margin-top: 6px; }
.cpw-btn { flex: 1; padding: 11px; font-size: 14px; font-weight: 600; border: none; border-radius: 10px; cursor: pointer; transition: all 0.15s; }
.cpw-btn.save { background: #1a1a1a; color: #fff; }
.cpw-btn.save:hover { background: #333; }
.cpw-btn.cancel { background: #f1f0eb; color: #555; }
.cpw-btn.cancel:hover { background: #e0ddd6; }

/* Change password button in topbar */
.cpw-topbar-btn { font-size: 12px; padding: 5px 12px; border-radius: 20px; border: 1px solid #ddd; cursor: pointer; font-weight: 500; background: #f5f4f0; color: #555; }
.cpw-topbar-btn:hover { background: #1a1a1a; color: #fff; border-color: #1a1a1a; }

/* ── USER SELECTOR SCREEN ── */
#user-select-screen {
  min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center;
  background: linear-gradient(135deg, #f5f4f0 0%, #ede9e0 100%); padding: 40px 20px;
}
.user-select-title { font-size: 22px; font-weight: 700; color: #1a1a1a; margin-bottom: 8px; text-align: center; }
.user-select-sub { font-size: 14px; color: #888; margin-bottom: 40px; text-align: center; }
.user-cards { display: flex; gap: 20px; flex-wrap: wrap; justify-content: center; }
.user-card {
  background: #fff; border: 2px solid #e8e6e0; border-radius: 20px; padding: 32px 36px;
  text-align: center; cursor: pointer; transition: all 0.2s; min-width: 180px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.06);
}
.user-card:hover { border-color: #1a1a1a; transform: translateY(-4px); box-shadow: 0 12px 32px rgba(0,0,0,0.12); }
.user-card.priyanka:hover { border-color: #db2777; }
.user-card.pravin:hover { border-color: #2563eb; }
.user-card.admin:hover { border-color: #d97706; }
.user-card.manish:hover { border-color: #7c3aed; }

/* ── USERNAME EDIT ── */
.edit-username-btn { font-size: 11px; background: none; border: 1px dashed #ccc; border-radius: 8px; padding: 2px 7px; cursor: pointer; color: #aaa; margin-left: 6px; }
.edit-username-btn:hover { border-color: #1a1a1a; color: #1a1a1a; }
.username-input { font-size: 17px; font-weight: 600; border: 1.5px solid #ddd; border-radius: 8px; padding: 3px 10px; font-family: inherit; background: #fafafa; }
.username-input:focus { outline: none; border-color: #1a1a1a; }

/* ── MANISH DASHBOARD ── */
#manish-dashboard { display: none; }
.m-accent { background: linear-gradient(90deg, #4c1d95 0%, #7c3aed 100%); height: 3px; }
.pw-submit.manish-btn { background: #7c3aed; color: #fff; }
.pw-submit.manish-btn:hover { background: #6d28d9; }
.m-tabs { display: flex; gap: 4px; background: #fff; border-radius: 12px; padding: 5px; border: 1px solid #e8e6e0; margin-bottom: 18px; flex-wrap: wrap; }
.m-tab { flex: 1; padding: 9px 6px; font-size: 12px; font-weight: 500; cursor: pointer; border: none; background: none; color: #888; border-radius: 8px; transition: all 0.15s; white-space: nowrap; min-width: 70px; text-align: center; }
.m-tab.active { background: #7c3aed; color: #fff; }
.m-tab:hover:not(.active) { background: #ede9fe; color: #7c3aed; }
.m-stats { display: grid; grid-template-columns: repeat(4,1fr); gap: 10px; margin-bottom: 20px; }
.m-stat { background: #fff; border-radius: 12px; padding: 14px 10px; text-align: center; border: 1px solid #e8e6e0; }
.m-stat-num { font-size: 22px; font-weight: 700; color: #7c3aed; }
.m-stat-label { font-size: 10px; color: #999; margin-top: 2px; text-transform: uppercase; letter-spacing: 0.5px; }
.m-project-card { background: #fff; border: 1.5px solid #e8e6e0; border-radius: 14px; padding: 16px; margin-bottom: 12px; position: relative; transition: box-shadow 0.2s; }
.m-project-card:hover { box-shadow: 0 4px 18px rgba(0,0,0,0.09); }
.m-proj-header { display: flex; align-items: flex-start; gap: 12px; margin-bottom: 12px; }
.m-proj-icon { font-size: 26px; flex-shrink: 0; }
.m-proj-info { flex: 1; min-width: 0; }
.m-proj-name { font-size: 15px; font-weight: 700; }
.m-proj-meta { font-size: 11px; color: #888; margin-top: 3px; display: flex; flex-wrap: wrap; gap: 6px; }
.m-proj-actions { display: flex; gap: 6px; }
.m-status-pill { font-size: 10px; padding: 2px 10px; border-radius: 20px; font-weight: 700; display: inline-block; }
.m-pill-pending  { background: #fef3c7; color: #92400e; }
.m-pill-upcoming { background: #dbeafe; color: #1d4ed8; }
.m-pill-current  { background: #dcfce7; color: #166534; }
.m-pill-completed{ background: #f3e8ff; color: #6d28d9; }
.m-pill-onhold   { background: #fee2e2; color: #991b1b; }
.m-deadline-row { display: flex; align-items: center; gap: 8px; margin-bottom: 8px; flex-wrap: wrap; }
.m-deadline-badge { font-size: 11px; padding: 3px 10px; border-radius: 20px; font-weight: 600; }
.m-deadline-ok     { background: #dcfce7; color: #166534; }
.m-deadline-warn   { background: #fef3c7; color: #92400e; }
.m-deadline-overdue{ background: #fee2e2; color: #991b1b; }
.m-deadline-none   { background: #f1f0eb; color: #888; }
.m-progress-row { display: flex; align-items: center; gap: 8px; margin-bottom: 10px; }
.m-progress-bar-bg { flex: 1; background: #f0ede8; border-radius: 10px; height: 8px; overflow: hidden; }
.m-progress-bar-fill { height: 100%; border-radius: 10px; background: linear-gradient(90deg, #7c3aed, #a78bfa); transition: width 0.4s; }
.m-progress-pct { font-size: 12px; font-weight: 700; color: #7c3aed; min-width: 34px; text-align: right; }
.m-tasks-section { border-top: 1px solid #f0ede8; padding-top: 10px; margin-top: 4px; }
.m-tasks-title { font-size: 11px; font-weight: 700; color: #888; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center; }
.m-task-row { display: flex; align-items: center; gap: 8px; padding: 7px 0; border-bottom: 1px solid #f9f8f6; }
.m-task-row:last-child { border-bottom: none; }
.m-task-check { width: 20px; height: 20px; border-radius: 50%; border: 2px solid #ddd; background: none; cursor: pointer; flex-shrink: 0; display: flex; align-items: center; justify-content: center; font-size: 11px; transition: all 0.15s; }
.m-task-check.done { background: #7c3aed; border-color: #7c3aed; color: #fff; }
.m-task-name { flex: 1; font-size: 13px; transition: color 0.15s; }
.m-task-name.done { text-decoration: line-through; color: #bbb; }
.m-task-deadline { font-size: 10px; color: #aaa; white-space: nowrap; }
.m-task-del { background: none; border: none; cursor: pointer; color: #ddd; font-size: 13px; padding: 2px 4px; border-radius: 4px; }
.m-task-del:hover { color: #ef4444; background: #fef2f2; }
.m-discussion-section { margin-top: 10px; }
.m-discussion-title { font-size: 11px; font-weight: 700; color: #888; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 8px; }
.m-discussion-note { background: #faf5ff; border: 1px solid #e9d5ff; border-radius: 10px; padding: 8px 12px; margin-bottom: 6px; display: flex; justify-content: space-between; gap: 8px; align-items: flex-start; }
.m-note-text { font-size: 12px; color: #4c1d95; flex: 1; }
.m-note-meta { font-size: 10px; color: #aaa; white-space: nowrap; }
.m-note-del { background: none; border: none; cursor: pointer; color: #ddd; font-size: 12px; }
.m-note-del:hover { color: #ef4444; }
.m-add-note-row { display: flex; gap: 6px; margin-top: 6px; }
.m-note-input { flex: 1; padding: 8px 10px; font-size: 12px; border: 1px solid #ddd; border-radius: 10px; font-family: inherit; background: #fafafa; }
.m-note-input:focus { outline: none; border-color: #7c3aed; }
.m-note-add-btn { padding: 7px 14px; font-size: 12px; font-weight: 600; border: none; border-radius: 10px; background: #7c3aed; color: #fff; cursor: pointer; white-space: nowrap; }
.m-note-add-btn:hover { background: #6d28d9; }
.m-add-task-row { display: flex; gap: 6px; margin-top: 8px; flex-wrap: wrap; }
.m-task-input { flex: 1; min-width: 120px; padding: 7px 10px; font-size: 12px; border: 1px solid #ddd; border-radius: 10px; font-family: inherit; background: #fafafa; }
.m-task-input:focus { outline: none; border-color: #7c3aed; }
.m-task-date-input { padding: 7px 8px; font-size: 12px; border: 1px solid #ddd; border-radius: 10px; font-family: inherit; background: #fafafa; }
.m-task-add-btn { padding: 7px 14px; font-size: 12px; font-weight: 600; border: none; border-radius: 10px; background: #7c3aed; color: #fff; cursor: pointer; }
.m-task-add-btn:hover { background: #6d28d9; }
.m-pct-row { display: flex; align-items: center; gap: 8px; }
.m-pct-input { width: 60px; padding: 4px 8px; font-size: 13px; font-weight: 700; border: 1.5px solid #ddd; border-radius: 8px; font-family: inherit; text-align: center; color: #7c3aed; }
.m-pct-input:focus { outline: none; border-color: #7c3aed; }
.m-proj-add-modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.4); z-index: 2000; align-items: flex-end; justify-content: center; }
.m-proj-add-modal.open { display: flex; }
.m-modal-body { background: #fff; border-radius: 20px 20px 0 0; padding: 20px 20px 36px; width: 100%; max-width: 700px; animation: slideup 0.22s ease; max-height: 90vh; overflow-y: auto; }
.m-form-group { display: flex; flex-direction: column; gap: 5px; margin-bottom: 14px; }
.m-form-group label { font-size: 11px; font-weight: 700; color: #666; text-transform: uppercase; letter-spacing: 0.5px; }
.m-form-input { padding: 10px 12px; font-size: 14px; border: 1.5px solid #ddd; border-radius: 10px; font-family: inherit; background: #fafafa; color: #1a1a1a; transition: border-color 0.15s; width: 100%; }
.m-form-input:focus { outline: none; border-color: #7c3aed; }
.m-submit-btn { width: 100%; padding: 12px; font-size: 14px; font-weight: 700; border: none; border-radius: 12px; cursor: pointer; background: #7c3aed; color: #fff; margin-top: 4px; }
.m-submit-btn:hover { background: #6d28d9; }
.m-submit-btn.secondary { background: #f1f0eb; color: #555; margin-top: 8px; }

/* ── PROJECT STATUS in Pravin ── */
.ps-proj-card { background: #fff; border: 1.5px solid #e8e6e0; border-radius: 14px; margin-bottom: 12px; overflow: hidden; cursor: pointer; transition: box-shadow 0.2s; }
.ps-proj-card:hover { box-shadow: 0 4px 18px rgba(0,0,0,0.09); }
.ps-proj-header { display: flex; align-items: center; gap: 12px; padding: 14px 16px; }
.ps-proj-header.open-bg { background: #eff6ff; }
.ps-proj-left { flex: 1; min-width: 0; }
.ps-proj-name { font-size: 14px; font-weight: 700; }
.ps-proj-meta { font-size: 11px; color: #888; margin-top: 2px; }
.ps-progress-row { display: flex; align-items: center; gap: 8px; margin-top: 6px; }
.ps-bar-bg { flex: 1; background: #f0ede8; border-radius: 10px; height: 7px; overflow: hidden; }
.ps-bar-fill { height: 100%; border-radius: 10px; background: linear-gradient(90deg, #2563eb, #60a5fa); transition: width 0.4s; }
.ps-pct { font-size: 11px; font-weight: 700; color: #2563eb; min-width: 30px; }
.ps-chevron { font-size: 12px; color: #aaa; transition: transform 0.2s; flex-shrink: 0; }
.ps-proj-card.ps-open .ps-chevron { transform: rotate(180deg); }
.ps-proj-body { padding: 0 16px 14px; border-top: 1px solid #e8e6e0; }
.ps-task-row { display: flex; align-items: center; gap: 8px; padding: 8px 0; border-bottom: 1px dashed #f0ede8; }
.ps-task-row:last-child { border-bottom: none; }
.ps-check { width: 22px; height: 22px; border-radius: 50%; border: 2px solid #bfdbfe; background: none; cursor: pointer; flex-shrink: 0; display: flex; align-items: center; justify-content: center; font-size: 12px; transition: all 0.15s; }
.ps-check.done { background: #2563eb; border-color: #2563eb; color: #fff; }
.ps-task-name { flex: 1; font-size: 13px; }
.ps-task-name.done { text-decoration: line-through; color: #bbb; }
.ps-task-date { font-size: 10px; color: #aaa; }
.ps-add-task-row { display: flex; gap: 6px; margin-top: 10px; flex-wrap: wrap; }
.ps-task-input { flex: 1; min-width: 100px; padding: 7px 10px; font-size: 12px; border: 1px solid #ddd; border-radius: 10px; font-family: inherit; }
.ps-task-input:focus { outline: none; border-color: #2563eb; }
.ps-task-date-input { padding: 7px 8px; font-size: 12px; border: 1px solid #ddd; border-radius: 10px; font-family: inherit; }
.ps-task-add-btn { padding: 7px 14px; font-size: 12px; font-weight: 600; border: none; border-radius: 10px; background: #2563eb; color: #fff; cursor: pointer; }
.ps-disc-section { margin-top: 10px; border-top: 1px solid #f0ede8; padding-top: 10px; }
.ps-disc-title { font-size: 11px; font-weight: 700; color: #888; text-transform: uppercase; letter-spacing: 0.4px; margin-bottom: 6px; }
.ps-disc-note { font-size: 12px; background: #eff6ff; border-radius: 8px; padding: 6px 10px; margin-bottom: 5px; color: #1d4ed8; display: flex; justify-content: space-between; gap: 6px; }
.ps-disc-meta { font-size: 10px; color: #aaa; white-space: nowrap; }
.ps-disc-del { background: none; border: none; cursor: pointer; color: #ddd; font-size: 11px; }
.ps-disc-del:hover { color: #ef4444; }
.ps-add-note-row { display: flex; gap: 6px; margin-top: 6px; }
.ps-note-input { flex: 1; padding: 7px 10px; font-size: 12px; border: 1px solid #ddd; border-radius: 10px; font-family: inherit; }
.ps-note-input:focus { outline: none; border-color: #2563eb; }
.ps-note-add-btn { padding: 7px 14px; font-size: 12px; font-weight: 600; border: none; border-radius: 10px; background: #2563eb; color: #fff; cursor: pointer; }
.ps-pct-row { display: flex; align-items: center; gap: 8px; margin: 8px 0; }
.ps-pct-input { width: 64px; padding: 4px 8px; font-size: 13px; font-weight: 700; border: 1.5px solid #bfdbfe; border-radius: 8px; text-align: center; font-family: inherit; color: #2563eb; }
.ps-pct-input:focus { outline: none; border-color: #2563eb; }

/* ── WORK ITEMS TAB (Pravin) ── */
.wi-section-header { display:flex; align-items:center; justify-content:space-between; margin-bottom:14px; flex-wrap:wrap; gap:8px; }
.wi-section-header h3 { font-size:15px; font-weight:700; }
.wi-filter-row { display:flex; gap:8px; flex-wrap:wrap; margin-bottom:14px; align-items:center; }

/* Work Item Row Card */
.wi-item-card { background:#fff; border:1.5px solid #e8e6e0; border-radius:14px; margin-bottom:10px; overflow:hidden; transition:box-shadow 0.2s; }
.wi-item-card:hover { box-shadow:0 4px 18px rgba(0,0,0,0.08); }
.wi-item-header { display:flex; align-items:flex-start; gap:12px; padding:14px 16px 10px; cursor:pointer; user-select:none; }
.wi-item-left { flex:1; min-width:0; }
.wi-item-name { font-size:14px; font-weight:700; color:#1a1a1a; }
.wi-item-desc { font-size:11px; color:#888; margin-top:2px; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }
.wi-item-meta { display:flex; flex-wrap:wrap; gap:6px; margin-top:5px; align-items:center; }
.wi-badge { font-size:10px; padding:2px 9px; border-radius:20px; font-weight:700; display:inline-block; }
.wi-badge-cat  { background:#dbeafe; color:#1d4ed8; }
.wi-badge-type-site     { background:#dcfce7; color:#166534; }
.wi-badge-type-prod     { background:#fef3c7; color:#92400e; }
.wi-badge-type-raw      { background:#f3e8ff; color:#6d28d9; }
.wi-badge-status-conf   { background:#dcfce7; color:#166534; }
.wi-badge-status-draft  { background:#f1f0eb; color:#555; }
.wi-badge-uom  { background:#f0f9ff; color:#0369a1; border:1px solid #bae6fd; }
.wi-qty-chip { font-size:11px; font-weight:700; color:#1a1a1a; background:#f1f0eb; border-radius:8px; padding:2px 8px; }
.wi-item-actions { display:flex; gap:4px; flex-shrink:0; }

/* Progress status label */
.wi-progress-label { font-size:11px; font-weight:700; color:#2563eb; padding:0 16px 4px; }
.wi-progress-ts { font-size:10px; color:#aaa; margin-left:auto; padding-right:16px; }

/* Stepped milestone bar */
.wi-milestone-wrap { padding:0 16px 14px; overflow-x:auto; }
.wi-milestone-track { position:relative; display:flex; align-items:flex-start; gap:0; min-width:600px; }
.wi-milestone-track::before {
  content:''; position:absolute; top:10px; left:10px; right:10px; height:3px;
  background:#e8e6e0; z-index:0;
}
.wi-milestone-fill {
  position:absolute; top:10px; left:10px; height:3px;
  background:linear-gradient(90deg,#2563eb,#38bdf8); z-index:1; transition:width 0.5s ease;
}
.wi-milestone-step { flex:1; display:flex; flex-direction:column; align-items:center; position:relative; z-index:2; cursor:pointer; }
.wi-step-dot {
  width:19px; height:19px; border-radius:50%; border:2px solid #e8e6e0;
  background:#fff; display:flex; align-items:center; justify-content:center;
  font-size:9px; transition:all 0.2s; flex-shrink:0;
}
.wi-step-dot.done   { background:#2563eb; border-color:#2563eb; color:#fff; }
.wi-step-dot.active { background:#fff; border-color:#2563eb; box-shadow:0 0 0 3px #bfdbfe; }
.wi-step-dot.done::after { content:"✓"; font-size:9px; font-weight:700; }
.wi-step-label { font-size:8px; color:#aaa; text-align:center; margin-top:4px; line-height:1.2; max-width:42px; word-break:break-word; }
.wi-step-label.done   { color:#2563eb; font-weight:600; }
.wi-step-label.active { color:#1a1a1a; font-weight:700; }

/* Simple % bar mode */
.wi-pct-bar-wrap { padding:0 16px 12px; }
.wi-pct-bar-track { background:#f0ede8; border-radius:10px; height:8px; overflow:hidden; margin-bottom:4px; }
.wi-pct-bar-fill { height:100%; border-radius:10px; background:linear-gradient(90deg,#2563eb,#60a5fa); transition:width 0.4s; }
.wi-pct-bar-labels { display:flex; justify-content:space-between; }
.wi-pct-bar-label { font-size:9px; color:#bbb; }
.wi-pct-bar-label.active { color:#2563eb; font-weight:700; }

/* Expanded body */
.wi-item-body { border-top:1px solid #f0ede8; padding:14px 16px; }
.wi-body-section { margin-bottom:12px; }
.wi-body-title { font-size:10px; font-weight:700; color:#888; text-transform:uppercase; letter-spacing:0.5px; margin-bottom:8px; }
.wi-tasks-list { display:flex; flex-direction:column; gap:6px; }
.wi-task-row { display:flex; align-items:center; gap:8px; padding:7px 10px; background:#f9f8f6; border-radius:10px; }
.wi-task-check { width:20px; height:20px; border-radius:50%; border:2px solid #bfdbfe; background:none; cursor:pointer; flex-shrink:0; display:flex; align-items:center; justify-content:center; font-size:11px; transition:all 0.15s; }
.wi-task-check.done { background:#2563eb; border-color:#2563eb; color:#fff; }
.wi-task-name { flex:1; font-size:12px; }
.wi-task-name.done { text-decoration:line-through; color:#bbb; }
.wi-task-date { font-size:10px; color:#aaa; white-space:nowrap; }
.wi-task-del { background:none; border:none; cursor:pointer; color:#ddd; font-size:12px; padding:2px 4px; }
.wi-task-del:hover { color:#ef4444; }
.wi-add-task-row { display:flex; gap:6px; margin-top:8px; flex-wrap:wrap; }
.wi-task-inp { flex:1; min-width:100px; padding:7px 10px; font-size:12px; border:1px solid #ddd; border-radius:10px; font-family:inherit; background:#fafafa; }
.wi-task-inp:focus { outline:none; border-color:#2563eb; }
.wi-task-date-inp { padding:7px 8px; font-size:12px; border:1px solid #ddd; border-radius:10px; font-family:inherit; background:#fafafa; }
.wi-task-add-btn { padding:7px 14px; font-size:12px; font-weight:600; border:none; border-radius:10px; background:#2563eb; color:#fff; cursor:pointer; white-space:nowrap; }
.wi-task-add-btn:hover { background:#1d4ed8; }

/* Milestone selector in expanded body */
.wi-milestone-select { width:100%; padding:8px 12px; font-size:13px; border:1.5px solid #bfdbfe; border-radius:10px; font-family:inherit; color:#1d4ed8; background:#eff6ff; font-weight:600; cursor:pointer; }
.wi-milestone-select:focus { outline:none; border-color:#2563eb; }
.wi-pct-manual-row { display:flex; align-items:center; gap:10px; flex-wrap:wrap; }
.wi-pct-inp { width:70px; padding:5px 8px; font-size:14px; font-weight:700; border:1.5px solid #bfdbfe; border-radius:8px; text-align:center; font-family:inherit; color:#2563eb; }
.wi-pct-inp:focus { outline:none; border-color:#2563eb; }
.wi-pct-auto-tag { font-size:11px; color:#aaa; }

/* Description edit textarea in expanded body */
.wi-desc-edit { width:100%; padding:8px 10px; font-size:13px; border:1.5px solid #bfdbfe; border-radius:10px; font-family:inherit; background:#f0f6ff; color:#1a1a1a; resize:vertical; transition:border-color 0.15s; }
.wi-desc-edit:focus { outline:none; border-color:#2563eb; background:#fff; }

/* Notes */
.wi-note-row { display:flex; justify-content:space-between; gap:8px; padding:7px 10px; background:#eff6ff; border-radius:9px; margin-bottom:5px; }
.wi-note-text { font-size:12px; color:#1d4ed8; flex:1; }
.wi-note-meta { font-size:10px; color:#aaa; white-space:nowrap; }
.wi-note-del { background:none; border:none; cursor:pointer; color:#ddd; font-size:11px; }
.wi-note-del:hover { color:#ef4444; }
.wi-add-note-row { display:flex; gap:6px; margin-top:6px; }
.wi-note-inp { flex:1; padding:7px 10px; font-size:12px; border:1px solid #ddd; border-radius:10px; font-family:inherit; }
.wi-note-inp:focus { outline:none; border-color:#2563eb; }
.wi-note-add-btn { padding:7px 14px; font-size:12px; font-weight:600; border:none; border-radius:10px; background:#2563eb; color:#fff; cursor:pointer; white-space:nowrap; }
.wi-note-add-btn:hover { background:#1d4ed8; }

/* Stat summary */
.wi-stats { display:grid; grid-template-columns:repeat(4,1fr); gap:10px; margin-bottom:18px; }
.wi-stat { background:#fff; border-radius:12px; padding:12px 8px; text-align:center; border:1px solid #e8e6e0; }
.wi-stat-num { font-size:20px; font-weight:700; color:#2563eb; }
.wi-stat-label { font-size:9px; color:#999; margin-top:2px; text-transform:uppercase; letter-spacing:0.5px; }
@media(max-width:480px){ .wi-stats{ grid-template-columns:repeat(2,1fr); } }

/* Mobile responsive overrides */
@media(max-width:600px){
  .topbar { padding: 10px 12px; flex-wrap: wrap; gap: 6px; }
  .topbar h1 { font-size: 15px; }
  .topbar-right { gap: 6px; flex-wrap: wrap; }
  .switch-user-btn, .cpw-topbar-btn { font-size: 11px; padding: 4px 9px; }
  .container { padding: 12px 10px; }
  .user-card { padding: 20px 18px; min-width: 130px; }
  .user-cards { gap: 12px; }
  .p-stats, .a-stats, .m-stats { grid-template-columns: repeat(2,1fr); }
  .stats { grid-template-columns: repeat(2,1fr); }
  .p-tabs, .m-tabs, .a-tabs, .tabs { gap: 2px; }
  .p-tab, .m-tab, .a-tab, .tab { font-size: 11px; padding: 7px 4px; min-width: 60px; }
  .excel-table { font-size: 11px; }
  .excel-table th, .excel-table td { padding: 6px 8px; }
  .modal { border-radius: 16px 16px 0 0; padding: 16px 14px 30px; }
  .pw-card { padding: 28px 18px; }
  .report-sum-card { min-width: 70px; }
  .m-project-card { padding: 12px; }
  .m-add-task-row, .ps-add-task-row, .m-add-note-row, .ps-add-note-row { flex-direction: column; }
}
#admin-dashboard { display: none; }
.admin-topbar-accent { background: linear-gradient(90deg, #92400e 0%, #d97706 100%); height: 3px; }
.a-tabs { display: flex; gap: 4px; background: #fff; border-radius: 12px; padding: 5px; border: 1px solid #e8e6e0; margin-bottom: 18px; flex-wrap: wrap; }
.a-tab { flex: 1; padding: 9px 6px; font-size: 12px; font-weight: 500; cursor: pointer; border: none; background: none; color: #888; border-radius: 8px; transition: all 0.15s; white-space: nowrap; min-width: 80px; text-align: center; }
.a-tab.active { background: #d97706; color: #fff; }
.a-tab:hover:not(.active) { background: #fef3c7; color: #92400e; }
.a-stats { display: grid; grid-template-columns: repeat(4,1fr); gap: 10px; margin-bottom: 20px; }
.a-stat { background: #fff; border-radius: 12px; padding: 14px 10px; text-align: center; border: 1px solid #e8e6e0; }
.a-stat-num { font-size: 22px; font-weight: 700; color: #d97706; }
.a-stat-label { font-size: 10px; color: #999; margin-top: 2px; text-transform: uppercase; letter-spacing: 0.5px; }
.admin-project-section { margin-bottom: 24px; }
.admin-project-header { display: flex; align-items: center; gap: 10px; padding: 12px 16px; background: #fffbeb; border: 1.5px solid #fcd34d; border-radius: 12px; margin-bottom: 10px; cursor: pointer; user-select: none; }
.admin-project-header h4 { font-size: 14px; font-weight: 700; color: #92400e; flex: 1; }
.admin-project-header .proj-order-count { font-size: 11px; background: #fef3c7; color: #92400e; border-radius: 20px; padding: 2px 10px; font-weight: 600; }
.admin-project-header .proj-chevron { font-size: 12px; color: #aaa; transition: transform 0.2s; }
.admin-project-header.collapsed .proj-chevron { transform: rotate(-90deg); }
.admin-order-card { background: #fff; border: 1px solid #e8e6e0; border-radius: 12px; padding: 14px 16px; margin-bottom: 8px; }
.admin-order-top { display: flex; align-items: flex-start; justify-content: space-between; gap: 10px; }
.admin-order-meta { font-size: 11px; color: #aaa; margin-top: 3px; }
.admin-order-vendor { font-size: 11px; color: #888; font-weight: 500; }
.admin-materials { display: flex; flex-direction: column; gap: 4px; margin: 10px 0; }
.admin-mat-row { font-size: 13px; color: #444; padding: 5px 10px; background: #f9f8f6; border-radius: 7px; display: flex; justify-content: space-between; }
.admin-action-row { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; margin-top: 10px; padding-top: 10px; border-top: 1px solid #f0ede8; }
.btn-approve { font-size: 12px; padding: 6px 16px; border-radius: 20px; border: none; cursor: pointer; font-weight: 700; background: #dcfce7; color: #166534; transition: all 0.12s; }
.btn-approve:hover { background: #16a34a; color: #fff; }
.btn-deny { font-size: 12px; padding: 6px 16px; border-radius: 20px; border: none; cursor: pointer; font-weight: 700; background: #fee2e2; color: #991b1b; transition: all 0.12s; }
.btn-deny:hover { background: #dc2626; color: #fff; }
.status-approved { background: #dcfce7; color: #166534; }
.status-denied { background: #fee2e2; color: #991b1b; }
.admin-note-row { margin-top: 8px; }
.admin-deny-reason { width: 100%; padding: 7px 10px; font-size: 12px; border: 1px solid #fca5a5; border-radius: 8px; font-family: inherit; color: #991b1b; background: #fff5f5; resize: none; }
.admin-deny-reason:focus { outline: none; border-color: #dc2626; }
.admin-summary-badge { font-size: 10px; padding: 2px 8px; border-radius: 10px; font-weight: 700; letter-spacing: 0.4px; }
.admin-filter-bar { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 16px; align-items: center; }
.admin-filter-select { padding: 7px 12px; border: 1px solid #ddd; border-radius: 10px; font-size: 13px; font-family: inherit; background: #fff; color: #1a1a1a; cursor: pointer; }
.admin-filter-select:focus { outline: none; border-color: #d97706; }
.approval-history-card { background: #fff; border: 1px solid #e8e6e0; border-radius: 12px; padding: 14px 16px; margin-bottom: 8px; display: flex; align-items: center; gap: 12px; }
.approval-history-icon { font-size: 22px; }
.approval-history-body { flex: 1; min-width: 0; }
.approval-history-title { font-size: 13px; font-weight: 600; }
.approval-history-sub { font-size: 11px; color: #aaa; margin-top: 2px; }
.user-avatar { font-size: 52px; margin-bottom: 12px; display: block; }
.user-card-name { font-size: 18px; font-weight: 700; color: #1a1a1a; }
.user-card-role { font-size: 12px; color: #999; margin-top: 4px; }

/* ── COMMON TOPBAR ── */
.topbar { background: #fff; border-bottom: 1px solid #e8e6e0; padding: 14px 20px; display: flex; align-items: center; justify-content: space-between; position: sticky; top: 0; z-index: 10; }
.topbar h1 { font-size: 17px; font-weight: 600; }
.topbar p { font-size: 12px; color: #888; margin-top: 1px; }
.topbar-right { display: flex; align-items: center; gap: 10px; }
.switch-user-btn { font-size: 12px; padding: 5px 12px; border-radius: 20px; border: 1px solid #ddd; cursor: pointer; font-weight: 500; background: #f5f4f0; color: #555; }
.switch-user-btn:hover { background: #1a1a1a; color: #fff; border-color: #1a1a1a; }
.notif-status { font-size: 12px; padding: 5px 12px; border-radius: 20px; border: 1px solid #ddd; cursor: pointer; font-weight: 500; background: #fafafa; }
.notif-status.on { background: #dcfce7; color: #166534; border-color: #86efac; }
.notif-status.off { background: #fef3c7; color: #92400e; border-color: #fcd34d; }
.notif-status.blocked { background: #fee2e2; color: #991b1b; border-color: #fca5a5; cursor: default; }

.container { max-width: 700px; margin: 0 auto; padding: 20px 16px; }

/* ── PRIYANKA DASHBOARD (ERP) ── */
#priyanka-dashboard { display: none; }
.stats { display: grid; grid-template-columns: repeat(3,1fr); gap: 10px; margin-bottom: 20px; }
.stat { background: #fff; border-radius: 12px; padding: 14px; text-align: center; border: 1px solid #e8e6e0; }
.stat-num { font-size: 26px; font-weight: 600; }
.stat-label { font-size: 11px; color: #999; margin-top: 2px; text-transform: uppercase; letter-spacing: 0.5px; }

.tabs { display: flex; gap: 4px; background: #fff; border-radius: 12px; padding: 5px; border: 1px solid #e8e6e0; margin-bottom: 18px; }
.tab { flex: 1; padding: 9px 4px; font-size: 12px; font-weight: 500; cursor: pointer; border: none; background: none; color: #888; border-radius: 8px; transition: all 0.15s; white-space: nowrap; }
.tab.active { background: #1a1a1a; color: #fff; }
.tab:hover:not(.active) { background: #f0ede8; color: #1a1a1a; }

.filter-row { display: flex; gap: 6px; margin-bottom: 14px; flex-wrap: wrap; }
.filter-btn { padding: 5px 14px; font-size: 12px; border: 1px solid #ddd; border-radius: 20px; background: #fff; cursor: pointer; color: #666; font-weight: 500; transition: all 0.12s; }
.filter-btn.active { background: #1a1a1a; color: #fff; border-color: #1a1a1a; }

.item-list { display: flex; flex-direction: column; gap: 8px; }
.item { display: flex; align-items: center; gap: 12px; padding: 14px 16px; background: #fff; border: 1px solid #e8e6e0; border-radius: 12px; transition: opacity 0.2s; }
.item.done { opacity: 0.45; }
.item.due-soon { border-left: 3px solid #ef4444; animation: pulse-border 2s infinite; }
@keyframes pulse-border { 0%,100%{border-left-color:#ef4444} 50%{border-left-color:#fca5a5} }
.item.repeat-item { border-right: 3px solid #8b5cf6; }

.done-btn { width: 24px; height: 24px; border-radius: 50%; border: 1.5px solid #ccc; background: none; cursor: pointer; display: flex; align-items: center; justify-content: center; flex-shrink: 0; font-size: 13px; transition: all 0.15s; }
.done-btn:hover { border-color: #22c55e; background: #f0fdf4; }
.done-btn.checked { background: #22c55e; border-color: #22c55e; color: #fff; }

.item-body { flex: 1; min-width: 0; }
.item-title { font-size: 14px; font-weight: 500; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.item.done .item-title { text-decoration: line-through; color: #aaa; }
.item-meta { font-size: 12px; color: #999; margin-top: 3px; }
.due-soon-tag { font-size: 10px; background: #fef2f2; color: #ef4444; border-radius: 6px; padding: 1px 6px; font-weight: 600; margin-left: 5px; }

.badge { display: inline-block; font-size: 10px; padding: 2px 7px; border-radius: 6px; font-weight: 600; margin-left: 7px; text-transform: uppercase; letter-spacing: 0.4px; vertical-align: middle; }
.badge-meeting { background: #dbeafe; color: #1d4ed8; }
.badge-task { background: #dcfce7; color: #166534; }
.badge-reminder { background: #fef3c7; color: #92400e; }
.badge-repeat { background: #ede9fe; color: #6d28d9; }

.item-actions { display: flex; gap: 2px; flex-shrink: 0; }
.icon-btn { background: none; border: none; cursor: pointer; padding: 5px 6px; border-radius: 8px; font-size: 15px; line-height: 1; transition: all 0.12s; color: #bbb; }
.icon-btn:hover.edit { color: #1d4ed8; background: #dbeafe; }
.icon-btn:hover.del { color: #ef4444; background: #fef2f2; }

.empty { text-align: center; padding: 40px 16px; color: #bbb; font-size: 14px; }
.empty-icon { font-size: 36px; margin-bottom: 10px; }

.form-panel { background: #fff; border: 1px solid #e8e6e0; border-radius: 16px; padding: 20px; display: flex; flex-direction: column; gap: 14px; }
.form-group { display: flex; flex-direction: column; gap: 5px; }
.form-group label { font-size: 12px; font-weight: 600; color: #666; text-transform: uppercase; letter-spacing: 0.5px; }
.form-group input, .form-group select, .form-group textarea {
  font-size: 14px; padding: 10px 12px; border: 1px solid #ddd; border-radius: 10px;
  background: #fafafa; color: #1a1a1a; font-family: inherit; width: 100%; transition: border-color 0.15s;
}
.form-group input:focus, .form-group select:focus, .form-group textarea:focus { outline: none; border-color: #1a1a1a; background: #fff; }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.submit-btn { padding: 12px; font-size: 15px; font-weight: 600; border: none; border-radius: 10px; background: #1a1a1a; color: #fff; cursor: pointer; transition: background 0.15s; margin-top: 4px; }
.submit-btn:hover { background: #333; }
.submit-btn.secondary { background: #f1f0eb; color: #1a1a1a; }
.submit-btn.secondary:hover { background: #e0ddd6; }

.type-selector { display: grid; grid-template-columns: repeat(3,1fr); gap: 8px; }
.type-opt { padding: 10px; border: 1.5px solid #e0ddd6; border-radius: 10px; cursor: pointer; text-align: center; font-size: 13px; font-weight: 500; color: #666; transition: all 0.12s; background: #fafafa; }
.type-opt.selected-meeting { border-color: #1d4ed8; background: #dbeafe; color: #1d4ed8; }
.type-opt.selected-task { border-color: #166534; background: #dcfce7; color: #166534; }
.type-opt.selected-reminder { border-color: #92400e; background: #fef3c7; color: #92400e; }
.type-icon { font-size: 20px; display: block; margin-bottom: 4px; }

.qd-btn { padding:4px 10px; font-size:11px; font-weight:600; border:1px solid #ddd; border-radius:20px; background:#fafafa; color:#555; cursor:pointer; transition:all 0.12s; }
.qd-btn:hover { background:#1a1a1a; color:#fff; border-color:#1a1a1a; }

.repeat-box { background: #faf5ff; border: 1.5px solid #ddd6fe; border-radius: 12px; padding: 14px; display: flex; flex-direction: column; gap: 10px; }
.repeat-toggle-row { display: flex; align-items: center; justify-content: space-between; }
.repeat-toggle-label { font-size: 13px; font-weight: 600; color: #6d28d9; }
.toggle-switch { position: relative; display: inline-block; width: 40px; height: 22px; }
.toggle-switch input { opacity: 0; width: 0; height: 0; }
.toggle-slider { position: absolute; cursor: pointer; inset: 0; background: #ddd; border-radius: 22px; transition: 0.2s; }
.toggle-slider:before { content: ''; position: absolute; width: 16px; height: 16px; left: 3px; bottom: 3px; background: #fff; border-radius: 50%; transition: 0.2s; }
input:checked + .toggle-slider { background: #8b5cf6; }
input:checked + .toggle-slider:before { transform: translateX(18px); }
.repeat-fields { display: none; flex-direction: column; gap: 10px; }
.repeat-fields.show { display: flex; }

.alert-banner { position: fixed; top: 70px; left: 50%; transform: translateX(-50%); width: min(94%,460px); background: #fff; border: 1.5px solid #ef4444; border-radius: 14px; padding: 14px 16px; z-index: 999; box-shadow: 0 8px 24px rgba(0,0,0,0.12); display: none; animation: slidedown 0.25s ease; }
@keyframes slidedown { from{opacity:0;transform:translateX(-50%) translateY(-10px)} to{opacity:1;transform:translateX(-50%) translateY(0)} }
.alert-banner .alert-title { font-size: 14px; font-weight: 600; margin-bottom: 2px; }
.alert-banner .alert-sub { font-size: 12px; color: #888; }
.alert-banner .alert-actions { display: flex; gap: 8px; margin-top: 10px; }
.alert-banner button { flex: 1; padding: 8px; font-size: 12px; font-weight: 600; border-radius: 8px; cursor: pointer; border: none; }
.btn-dismiss { background: #f1f0eb; color: #666; }
.btn-done-alert { background: #22c55e; color: #fff; }

.modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.4); z-index: 2000; align-items: flex-end; justify-content: center; }
.modal-overlay.open { display: flex; }
.modal { background: #fff; border-radius: 20px 20px 0 0; padding: 20px 20px 36px; width: 100%; max-width: 700px; animation: slideup 0.22s ease; max-height: 92vh; overflow-y: auto; }
@keyframes slideup { from{transform:translateY(30px);opacity:0} to{transform:translateY(0);opacity:1} }
.modal-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 18px; }
.modal-header h2 { font-size: 16px; font-weight: 600; }
.modal-close { background: #f1f0eb; border: none; border-radius: 50%; width: 30px; height: 30px; font-size: 16px; cursor: pointer; color: #555; }
.modal-close:hover { background: #e0ddd6; }

.success-toast { position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%); background: #1a1a1a; color: #fff; padding: 10px 22px; border-radius: 20px; font-size: 13px; font-weight: 500; z-index: 9999; display: none; }

.persist-dock { position: fixed; bottom: 20px; right: 18px; z-index: 8000; display: flex; flex-direction: column; gap: 8px; align-items: flex-end; max-width: 310px; pointer-events: none; }
.persist-card { background: #fff; border: 1.5px solid #e8e6e0; border-radius: 14px; padding: 12px 14px; box-shadow: 0 6px 24px rgba(0,0,0,0.13); pointer-events: all; animation: slideInRight 0.28s ease; width: 300px; position: relative; }
.persist-card.urgent { border-left: 4px solid #ef4444; }
.persist-card.today  { border-left: 4px solid #f97316; }
.persist-card.week   { border-left: 4px solid #3b82f6; }
.persist-card.repeat-card { border-left: 4px solid #8b5cf6; }
@keyframes slideInRight { from{opacity:0;transform:translateX(30px)} to{opacity:1;transform:translateX(0)} }
.persist-card-top { display:flex; align-items:flex-start; gap:9px; }
.persist-icon { font-size:20px; flex-shrink:0; margin-top:1px; }
.persist-body { flex:1; min-width:0; }
.persist-title { font-size:13px; font-weight:600; color:#1a1a1a; line-height:1.3; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }
.persist-sub { font-size:11px; color:#888; margin-top:2px; }
.persist-urgency { font-size:10px; font-weight:700; padding:2px 7px; border-radius:20px; display:inline-block; margin-top:5px; text-transform:uppercase; letter-spacing:0.4px; }
.persist-urgency.u-urgent { background:#fef2f2; color:#ef4444; }
.persist-urgency.u-today  { background:#fff7ed; color:#ea580c; }
.persist-urgency.u-week   { background:#eff6ff; color:#2563eb; }
.persist-urgency.u-repeat { background:#ede9fe; color:#6d28d9; }
.persist-actions { display:flex; gap:6px; margin-top:10px; }
.persist-btn { flex:1; padding:7px 0; font-size:11px; font-weight:600; border:none; border-radius:8px; cursor:pointer; transition:all 0.12s; }
.persist-btn.snooze  { background:#f1f0eb; color:#555; }
.persist-btn.snooze:hover { background:#e0ddd6; }
.persist-btn.done-p  { background:#22c55e; color:#fff; }
.persist-btn.done-p:hover { background:#16a34a; }
.persist-close { position:absolute; top:8px; right:9px; background:none; border:none; font-size:13px; cursor:pointer; color:#ccc; line-height:1; padding:2px 4px; border-radius:6px; }
.persist-close:hover { color:#ef4444; background:#fef2f2; }
.persist-toggle { position:fixed; bottom:20px; right:18px; z-index:8001; background:#1a1a1a; color:#fff; border:none; border-radius:50px; padding:10px 16px; font-size:13px; font-weight:600; cursor:pointer; box-shadow:0 4px 16px rgba(0,0,0,0.2); display:none; align-items:center; gap:6px; }
.persist-toggle .badge-count { background:#ef4444; color:#fff; border-radius:50%; width:18px; height:18px; font-size:10px; font-weight:700; display:inline-flex; align-items:center; justify-content:center; }

.search-wrap { position: relative; margin-bottom: 14px; }
.search-wrap input { width: 100%; padding: 10px 36px 10px 38px; font-size: 14px; border: 1.5px solid #e0ddd6; border-radius: 12px; background: #fff; color: #1a1a1a; font-family: inherit; transition: border-color 0.15s; }
.search-wrap input:focus { outline: none; border-color: #1a1a1a; }
.search-icon { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); font-size: 15px; pointer-events: none; }
.search-clear { position: absolute; right: 10px; top: 50%; transform: translateY(-50%); background: #e0ddd6; border: none; border-radius: 50%; width: 20px; height: 20px; font-size: 11px; cursor: pointer; color: #555; display: none; align-items: center; justify-content: center; line-height: 1; }
.search-clear.visible { display: flex; }
.search-result-count { font-size: 11px; color: #aaa; margin-bottom: 8px; margin-top: -8px; padding-left: 2px; display: none; }
.search-result-count.visible { display: block; }

/* ── PRAVIN DASHBOARD ── */
#pravin-dashboard { display: none; }

/* Pravin accent = blue */
.p-tabs { display: flex; gap: 4px; background: #fff; border-radius: 12px; padding: 5px; border: 1px solid #e8e6e0; margin-bottom: 18px; flex-wrap: wrap; }
.p-tab { flex: 1; padding: 9px 6px; font-size: 12px; font-weight: 500; cursor: pointer; border: none; background: none; color: #888; border-radius: 8px; transition: all 0.15s; white-space: nowrap; min-width: 80px; text-align: center; }
.p-tab.active { background: #2563eb; color: #fff; }
.p-tab:hover:not(.active) { background: #eff6ff; color: #2563eb; }

.p-stats { display: grid; grid-template-columns: repeat(4,1fr); gap: 10px; margin-bottom: 20px; }
.p-stat { background: #fff; border-radius: 12px; padding: 14px 10px; text-align: center; border: 1px solid #e8e6e0; }
.p-stat-num { font-size: 22px; font-weight: 700; color: #2563eb; }
.p-stat-label { font-size: 10px; color: #999; margin-top: 2px; text-transform: uppercase; letter-spacing: 0.5px; }

/* Vendor / Project cards */
.card-grid { display: flex; flex-direction: column; gap: 10px; }
.p-card { background: #fff; border: 1px solid #e8e6e0; border-radius: 14px; padding: 16px; display: flex; align-items: center; gap: 14px; }
.p-card-icon { font-size: 28px; flex-shrink: 0; }
.p-card-body { flex: 1; min-width: 0; }
.p-card-name { font-size: 15px; font-weight: 600; }
.p-card-sub { font-size: 12px; color: #888; margin-top: 3px; }
.p-card-actions { display: flex; gap: 4px; flex-shrink: 0; }

/* Order list */
.order-card { background: #fff; border: 1px solid #e8e6e0; border-radius: 14px; padding: 16px; }
.order-card-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 10px; margin-bottom: 10px; }
.order-card-title { font-size: 14px; font-weight: 600; }
.order-card-project { font-size: 11px; color: #2563eb; font-weight: 600; margin-top: 2px; }
.order-card-vendor { font-size: 11px; color: #888; margin-top: 2px; }
.order-status-row { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; }
.status-badge { font-size: 10px; padding: 3px 10px; border-radius: 20px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.4px; }
.status-pending { background: #fef3c7; color: #92400e; }
.status-ordered { background: #dbeafe; color: #1d4ed8; }
.status-delivered { background: #dcfce7; color: #166534; }
.order-action-btn { font-size: 11px; padding: 4px 12px; border-radius: 20px; border: none; cursor: pointer; font-weight: 600; transition: all 0.12s; }
.btn-mark-ordered { background: #dbeafe; color: #1d4ed8; }
.btn-mark-ordered:hover { background: #2563eb; color: #fff; }
.btn-mark-delivered { background: #dcfce7; color: #166534; }
.btn-mark-delivered:hover { background: #16a34a; color: #fff; }
.btn-del-order { background: #fef2f2; color: #ef4444; }
.btn-del-order:hover { background: #ef4444; color: #fff; }

/* Admin status strip on Pravin's cards */
.admin-status-strip { display: flex; align-items: center; gap: 8px; padding: 7px 12px; border-radius: 8px; margin-top: 10px; font-size: 12px; font-weight: 600; }
.admin-strip-awaiting { background: #fef9ec; border: 1px solid #fcd34d; color: #92400e; }
.admin-strip-approved { background: #f0fdf4; border: 1px solid #86efac; color: #166534; }
.admin-strip-denied   { background: #fff1f2; border: 1px solid #fca5a5; color: #991b1b; }
.admin-strip-icon { font-size: 15px; }
.admin-strip-reason { font-size: 11px; font-weight: 400; color: #666; margin-top: 2px; }

.order-items-list { display: flex; flex-direction: column; gap: 4px; margin-top: 10px; }
.order-item-row { font-size: 13px; color: #444; padding: 6px 10px; background: #f9f8f6; border-radius: 8px; display: flex; justify-content: space-between; }
.order-qty { font-size: 12px; color: #888; }

.section-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
.section-header h3 { font-size: 15px; font-weight: 700; }

.add-inline-btn { font-size: 12px; padding: 6px 14px; border-radius: 20px; border: 1px solid #2563eb; background: #eff6ff; color: #2563eb; cursor: pointer; font-weight: 600; transition: all 0.12s; }
.add-inline-btn:hover { background: #2563eb; color: #fff; }

/* Material rows in add-order form */
.material-row { display: flex; gap: 8px; align-items: center; }
.material-row input { flex: 1; }
.material-row .qty-input { width: 80px; flex: none; }
.remove-mat-btn { background: #fef2f2; border: none; border-radius: 8px; color: #ef4444; cursor: pointer; padding: 8px 10px; font-size: 14px; font-weight: 700; }
.add-mat-btn { font-size: 12px; padding: 7px 14px; border-radius: 8px; border: 1px dashed #ddd; background: #fafafa; color: #666; cursor: pointer; font-weight: 500; width: 100%; transition: all 0.12s; }
.add-mat-btn:hover { border-color: #2563eb; color: #2563eb; background: #eff6ff; }

.p-submit-btn { padding: 12px; font-size: 15px; font-weight: 600; border: none; border-radius: 10px; background: #2563eb; color: #fff; cursor: pointer; transition: background 0.15s; margin-top: 4px; width: 100%; }
.p-submit-btn:hover { background: #1d4ed8; }
.p-submit-btn.secondary { background: #f1f0eb; color: #1a1a1a; }
.p-submit-btn.secondary:hover { background: #e0ddd6; }

.filter-select { padding: 8px 12px; border: 1px solid #ddd; border-radius: 10px; font-size: 13px; font-family: inherit; background: #fff; color: #1a1a1a; cursor: pointer; }
.filter-select:focus { outline: none; border-color: #2563eb; }

/* ── MATERIALS REPORT TAB ── */
.report-project-card { background: #fff; border: 1.5px solid #e8e6e0; border-radius: 14px; margin-bottom: 12px; overflow: hidden; transition: box-shadow 0.2s; }
.report-project-card:hover { box-shadow: 0 4px 18px rgba(0,0,0,0.09); }
.report-project-header { display: flex; align-items: center; gap: 12px; padding: 14px 16px; cursor: pointer; user-select: none; background: #fafaf8; border-bottom: 1.5px solid transparent; transition: background 0.15s; }
.report-project-card.open .report-project-header { background: #eff6ff; border-bottom-color: #bfdbfe; }
.report-project-header:hover { background: #f0f4ff; }
.report-proj-icon { font-size: 24px; flex-shrink: 0; }
.report-proj-info { flex: 1; min-width: 0; }
.report-proj-name { font-size: 15px; font-weight: 700; color: #1a1a1a; }
.report-proj-meta { font-size: 11px; color: #888; margin-top: 2px; }
.report-proj-badges { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 5px; }
.report-badge { font-size: 10px; padding: 2px 8px; border-radius: 10px; font-weight: 700; }
.report-badge-total  { background: #dbeafe; color: #1d4ed8; }
.report-badge-approved { background: #dcfce7; color: #166534; }
.report-badge-pending  { background: #fef3c7; color: #92400e; }
.report-badge-denied   { background: #fee2e2; color: #991b1b; }
.report-chevron { font-size: 13px; color: #aaa; transition: transform 0.22s; flex-shrink: 0; }
.report-project-card.open .report-chevron { transform: rotate(180deg); }
.report-export-row { display: flex; gap: 8px; align-items: center; padding: 10px 16px 10px; border-top: 1px solid #e8e6e0; background: #f9f8f6; flex-wrap: wrap; }
.report-export-btn { font-size: 12px; padding: 6px 14px; border-radius: 20px; border: none; cursor: pointer; font-weight: 600; transition: all 0.12s; display: flex; align-items: center; gap: 5px; }
.btn-export-excel { background: #166534; color: #fff; }
.btn-export-excel:hover { background: #14532d; }
.btn-export-csv { background: #1d4ed8; color: #fff; }
.btn-export-csv:hover { background: #1e40af; }
.btn-export-all { background: #d97706; color: #fff; }
.btn-export-all:hover { background: #b45309; }

/* Excel-style table */
.excel-table-wrap { overflow-x: auto; padding: 0 0 0 0; }
.excel-table { width: 100%; border-collapse: collapse; font-size: 12px; }
.excel-table th { background: #1d4ed8; color: #fff; padding: 9px 12px; text-align: left; font-weight: 700; font-size: 11px; letter-spacing: 0.4px; white-space: nowrap; border-right: 1px solid #2563eb; }
.excel-table th:last-child { border-right: none; }
.excel-table td { padding: 8px 12px; border-bottom: 1px solid #f0ede8; border-right: 1px solid #f0ede8; white-space: nowrap; color: #333; vertical-align: middle; }
.excel-table td:last-child { border-right: none; }
.excel-table tr:nth-child(even) td { background: #f8faff; }
.excel-table tr:hover td { background: #eff6ff; }
.excel-table .col-sno { color: #aaa; font-size: 11px; text-align: center; width: 36px; }
.excel-table .col-status { text-align: center; }
.excel-table .col-qty { text-align: right; font-weight: 600; color: #2563eb; }
.excel-table .subtotal-row td { background: #fffbeb !important; font-weight: 700; border-top: 2px solid #fcd34d; color: #92400e; font-size: 11px; }
.excel-status-pill { font-size: 10px; padding: 2px 8px; border-radius: 10px; font-weight: 700; display: inline-block; }
.pill-approved { background: #dcfce7; color: #166534; }
.pill-pending  { background: #fef3c7; color: #92400e; }
.pill-denied   { background: #fee2e2; color: #991b1b; }
.pill-ordered  { background: #dbeafe; color: #1d4ed8; }
.pill-delivered{ background: #f3e8ff; color: #6d28d9; }
.report-empty { text-align: center; padding: 30px; color: #bbb; font-size: 13px; }
.report-summary-bar { display: flex; gap: 10px; flex-wrap: wrap; margin-bottom: 14px; }
.report-sum-card { flex: 1; min-width: 100px; background: #fff; border: 1px solid #e8e6e0; border-radius: 10px; padding: 10px 12px; text-align: center; }
.report-sum-num { font-size: 20px; font-weight: 700; color: #2563eb; }
.report-sum-label { font-size: 10px; color: #999; text-transform: uppercase; letter-spacing: 0.4px; margin-top: 2px; }
.export-all-row { display: flex; justify-content: flex-end; margin-bottom: 14px; }

@media(max-width:480px){ .form-row{ grid-template-columns:1fr; } .p-stats{ grid-template-columns: repeat(2,1fr); } .user-cards{ flex-direction: column; } }
</style>
</head>
<body>

<!-- ══════════════ PASSWORD SCREEN ══════════════ -->
<div id="password-screen">
  <div class="pw-card">
    <span class="pw-avatar" id="pw-avatar">🌸</span>
    <div class="pw-name" id="pw-name">Priyanka</div>
    <div class="pw-sub">Enter your password to continue</div>
    <div class="pw-input-wrap">
      <input class="pw-input" type="password" id="pw-input" placeholder="Password" onkeydown="if(event.key==='Enter')submitPassword()">
      <button class="pw-eye" id="pw-eye" onclick="togglePwEye()" title="Show/hide">👁️</button>
    </div>
    <div class="pw-error" id="pw-error"></div>
    <button class="pw-submit" id="pw-submit" onclick="submitPassword()">Unlock →</button>
    <br>
    <button class="pw-back" onclick="showUserSelect()">← Back</button>
    <div class="pw-hint" id="pw-hint">Default password: <b>1234</b></div>
  </div>
</div>

<!-- ══════════════ CHANGE PASSWORD MODAL (shared) ══════════════ -->
<div class="cpw-modal-overlay" id="cpw-modal">
  <div class="cpw-modal">
    <h2>🔑 Change Password</h2>
    <div class="cpw-group">
      <label>Current Password</label>
      <div class="cpw-input-wrap">
        <input class="cpw-input" type="password" id="cpw-current" placeholder="Enter current password">
        <button class="cpw-eye" onclick="toggleCpwEye('cpw-current')">👁️</button>
      </div>
    </div>
    <div class="cpw-error" id="cpw-current-error"></div>
    <div class="cpw-group">
      <label>New Password</label>
      <div class="cpw-input-wrap">
        <input class="cpw-input" type="password" id="cpw-new" placeholder="Min 4 characters" oninput="updateStrength()">
        <button class="cpw-eye" onclick="toggleCpwEye('cpw-new')">👁️</button>
      </div>
      <div class="cpw-strength"><div class="cpw-strength-bar" id="cpw-strength-bar"></div></div>
      <div class="cpw-strength-label" id="cpw-strength-label"></div>
    </div>
    <div class="cpw-group">
      <label>Confirm New Password</label>
      <div class="cpw-input-wrap">
        <input class="cpw-input" type="password" id="cpw-confirm" placeholder="Re-enter new password">
        <button class="cpw-eye" onclick="toggleCpwEye('cpw-confirm')">👁️</button>
      </div>
    </div>
    <div class="cpw-error" id="cpw-error"></div>
    <div class="cpw-actions">
      <button class="cpw-btn cancel" onclick="closeCpwModal()">Cancel</button>
      <button class="cpw-btn save" id="cpw-save-btn" onclick="savePassword()">Save Password</button>
    </div>
  </div>
</div>

<!-- ══════════════ USER SELECT SCREEN ══════════════ -->
<div id="user-select-screen">
  <div class="user-select-title">👋 Welcome! Who are you?</div>
  <div class="user-select-sub">Choose your dashboard to continue</div>
  <div class="user-cards">
    <div class="user-card priyanka" onclick="showPasswordScreen('priyanka')">
      <span class="user-avatar">🌸</span>
      <div class="user-card-name" id="uname-priyanka">Priyanka</div>
      <div class="user-card-role">ERP Dashboard</div>
    </div>
    <div class="user-card pravin" onclick="showPasswordScreen('pravin')">
      <span class="user-avatar">🏗️</span>
      <div class="user-card-name" id="uname-pravin">Pravin</div>
      <div class="user-card-role">Projects & Orders</div>
    </div>
    <div class="user-card admin" onclick="showPasswordScreen('admin')">
      <span class="user-avatar">🛡️</span>
      <div class="user-card-name" id="uname-admin">Admin</div>
      <div class="user-card-role">Approve & Review Orders</div>
    </div>
    <div class="user-card manish" onclick="showPasswordScreen('manish')">
      <span class="user-avatar">✏️</span>
      <div class="user-card-name" id="uname-manish">Manish</div>
      <div class="user-card-role">Design Follow-up</div>
    </div>
  </div>
</div>

<!-- ══════════════ ALERT BANNER (shared) ══════════════ -->
<div class="alert-banner" id="alert-banner">
  <div style="display:flex;align-items:flex-start;gap:10px">
    <span style="font-size:22px">⏰</span>
    <div style="flex:1">
      <div class="alert-title" id="alert-title">Reminder</div>
      <div class="alert-sub" id="alert-sub"></div>
      <div class="alert-actions">
        <button class="btn-dismiss" onclick="dismissAlert()">Dismiss</button>
        <button class="btn-done-alert" onclick="doneFromAlert()">✓ Mark Done</button>
      </div>
    </div>
  </div>
</div>

<!-- ══════════════ PRIYANKA'S ERP DASHBOARD ══════════════ -->
<div id="priyanka-dashboard">
  <div class="topbar">
    <div>
      <h1>🌸 Priyanka's Dashboard</h1>
      <p id="today-date"></p>
    </div>
    <div class="topbar-right">
      <button class="switch-user-btn" onclick="showUserSelect()">⇄ Switch User</button>
      <button class="cpw-topbar-btn" onclick="openCpwModal()">🔑 Password</button>
      <button class="cpw-topbar-btn" onclick="openEditUsername('priyanka')">✏️ Name</button>
      <button class="notif-status off" id="notif-btn" onclick="requestNotifPermission()">🔔 Enable Alerts</button>
    </div>
  </div>

  <!-- Edit Modal -->
  <div class="modal-overlay" id="edit-modal" onclick="closeEditModal(event)">
    <div class="modal">
      <div class="modal-header">
        <h2>✏️ Edit Item</h2>
        <button class="modal-close" onclick="closeEditModal()">✕</button>
      </div>
      <div style="display:flex;flex-direction:column;gap:14px">
        <div class="form-group">
          <label>Type</label>
          <div class="type-selector">
            <div class="type-opt" id="eopt-meeting" onclick="selectEditType('meeting')"><span class="type-icon">📅</span>Meeting</div>
            <div class="type-opt" id="eopt-task" onclick="selectEditType('task')"><span class="type-icon">✅</span>Task</div>
            <div class="type-opt" id="eopt-reminder" onclick="selectEditType('reminder')"><span class="type-icon">🔔</span>Reminder</div>
          </div>
        </div>
        <div class="form-group"><label>Title *</label><input id="edit-title" type="text" placeholder="Title"></div>
        <div class="form-row">
          <div class="form-group">
            <label>Date</label>
            <input id="edit-date" type="date">
            <div style="display:flex;flex-wrap:wrap;gap:5px;margin-top:6px">
              <button type="button" onclick="setEditQuickDate(0)" class="qd-btn">Today</button>
              <button type="button" onclick="setEditQuickDate(1)" class="qd-btn">Tomorrow</button>
              <button type="button" onclick="setEditQuickDate(7)" class="qd-btn">In 1 week</button>
            </div>
          </div>
          <div class="form-group"><label>Time</label><input id="edit-time" type="time"></div>
        </div>
        <div class="form-group">
          <label>Alert me before</label>
          <select id="edit-remind">
            <option value="0">At exact time</option>
            <option value="5">5 minutes before</option>
            <option value="10">10 minutes before</option>
            <option value="15">15 minutes before</option>
            <option value="30">30 minutes before</option>
            <option value="60">1 hour before</option>
          </select>
        </div>
        <div class="form-group"><label>Note</label><input id="edit-note" type="text" placeholder="Any extra details..."></div>
        <div class="repeat-box">
          <div class="repeat-toggle-row">
            <span class="repeat-toggle-label">🔁 Repeat Reminder</span>
            <label class="toggle-switch">
              <input type="checkbox" id="edit-repeat-on" onchange="toggleEditRepeat(this)">
              <span class="toggle-slider"></span>
            </label>
          </div>
          <div class="repeat-fields" id="edit-repeat-fields">
            <div class="form-row">
              <div class="form-group"><label>Repeat Until</label><input id="edit-repeat-end" type="date"></div>
              <div class="form-group">
                <label>Repeat Every</label>
                <select id="edit-repeat-freq">
                  <option value="daily">Every day</option>
                  <option value="2days">Every 2 days</option>
                  <option value="weekly">Every week</option>
                </select>
              </div>
            </div>
          </div>
        </div>
        <button class="submit-btn" onclick="saveEdit()">💾 Save Changes</button>
        <button class="submit-btn secondary" onclick="closeEditModal()">Cancel</button>
      </div>
    </div>
  </div>

  <div class="container">
    <div class="stats">
      <div class="stat"><div class="stat-num" id="s-pending">0</div><div class="stat-label">Pending</div></div>
      <div class="stat"><div class="stat-num" id="s-today">0</div><div class="stat-label">Due Today</div></div>
      <div class="stat"><div class="stat-num" id="s-done">0</div><div class="stat-label">Completed</div></div>
    </div>
    <div class="tabs">
      <button class="tab active" onclick="switchTab('today')">📅 Today</button>
      <button class="tab" onclick="switchTab('all')">📋 All</button>
      <button class="tab" onclick="switchTab('done')">✅ Done</button>
      <button class="tab" onclick="switchTab('add')">➕ Add</button>
    </div>

    <!-- TODAY -->
    <div id="tab-today">
      <div class="search-wrap">
        <span class="search-icon">🔍</span>
        <input type="text" id="search-today" placeholder="Search today's items…" oninput="onSearch('today')" autocomplete="off">
        <button class="search-clear" id="search-clear-today" onclick="clearSearch('today')">✕</button>
      </div>
      <div class="search-result-count" id="search-count-today"></div>
      <div class="filter-row">
        <button class="filter-btn active" onclick="setFilter('all',this)">All</button>
        <button class="filter-btn" onclick="setFilter('meeting',this)">📅 Meetings</button>
        <button class="filter-btn" onclick="setFilter('task',this)">✅ Tasks</button>
        <button class="filter-btn" onclick="setFilter('reminder',this)">🔔 Reminders</button>
      </div>
      <div class="item-list" id="list-today"></div>
    </div>

    <!-- ALL -->
    <div id="tab-all" style="display:none">
      <div class="search-wrap">
        <span class="search-icon">🔍</span>
        <input type="text" id="search-all" placeholder="Search all items…" oninput="onSearch('all')" autocomplete="off">
        <button class="search-clear" id="search-clear-all" onclick="clearSearch('all')">✕</button>
      </div>
      <div class="search-result-count" id="search-count-all"></div>
      <div class="item-list" id="list-all"></div>
    </div>

    <!-- DONE -->
    <div id="tab-done" style="display:none">
      <div style="display:flex;justify-content:flex-end;margin-bottom:10px">
        <button onclick="clearDone()" style="font-size:12px;color:#ef4444;background:#fff;border:1px solid #fca5a5;border-radius:8px;padding:6px 14px;cursor:pointer;font-weight:500">🗑 Clear all done</button>
      </div>
      <div class="item-list" id="list-done"></div>
    </div>

    <!-- ADD -->
    <div id="tab-add" style="display:none">
      <div class="form-panel">
        <div class="form-group">
          <label>Type</label>
          <div class="type-selector">
            <div class="type-opt selected-meeting" id="opt-meeting" onclick="selectType('meeting')"><span class="type-icon">📅</span>Meeting</div>
            <div class="type-opt" id="opt-task" onclick="selectType('task')"><span class="type-icon">✅</span>Task</div>
            <div class="type-opt" id="opt-reminder" onclick="selectType('reminder')"><span class="type-icon">🔔</span>Reminder</div>
          </div>
        </div>
        <div class="form-group"><label>Title *</label><input id="new-title" type="text" placeholder="e.g. Doctor appointment, Pay bill..."></div>
        <div class="form-row">
          <div class="form-group">
            <label>Start Date</label>
            <input id="new-date" type="date">
            <div style="display:flex;flex-wrap:wrap;gap:5px;margin-top:6px">
              <button type="button" onclick="setQuickDate(0)" class="qd-btn">Today</button>
              <button type="button" onclick="setQuickDate(1)" class="qd-btn">Tomorrow</button>
              <button type="button" onclick="setQuickDate(7)" class="qd-btn">In 1 week</button>
            </div>
          </div>
          <div class="form-group"><label>Time</label><input id="new-time" type="time"></div>
        </div>
        <div class="form-group">
          <label>Alert me before</label>
          <select id="new-remind">
            <option value="0">At exact time</option>
            <option value="5">5 minutes before</option>
            <option value="10" selected>10 minutes before</option>
            <option value="15">15 minutes before</option>
            <option value="30">30 minutes before</option>
            <option value="60">1 hour before</option>
          </select>
        </div>
        <div class="form-group"><label>Note (optional)</label><input id="new-note" type="text" placeholder="Any extra details..."></div>
        <div class="repeat-box">
          <div class="repeat-toggle-row">
            <span class="repeat-toggle-label">🔁 Repeat Reminder</span>
            <label class="toggle-switch">
              <input type="checkbox" id="new-repeat-on" onchange="toggleNewRepeat(this)">
              <span class="toggle-slider"></span>
            </label>
          </div>
          <div class="repeat-fields" id="new-repeat-fields">
            <div class="form-row">
              <div class="form-group">
                <label>Repeat Until</label>
                <input id="new-repeat-end" type="date">
                <div style="display:flex;flex-wrap:wrap;gap:5px;margin-top:6px">
                  <button type="button" onclick="setRepeatEndQuick(7)" class="qd-btn">1 week</button>
                  <button type="button" onclick="setRepeatEndQuick(30)" class="qd-btn">1 month</button>
                </div>
              </div>
              <div class="form-group">
                <label>Remind Every</label>
                <select id="new-repeat-freq">
                  <option value="daily">Every day</option>
                  <option value="2days">Every 2 days</option>
                  <option value="weekly">Every week</option>
                </select>
              </div>
            </div>
          </div>
        </div>
        <button class="submit-btn" onclick="addItem()">Add to Dashboard ✓</button>
      </div>
    </div>
  </div>

  <div class="persist-dock" id="persist-dock"></div>
  <button class="persist-toggle" id="persist-toggle" onclick="expandDock()">⏰ Reminders <span class="badge-count" id="persist-count">0</span></button>
</div>
<!-- END PRIYANKA -->

<!-- ══════════════ PRAVIN'S DASHBOARD ══════════════ -->
<div id="pravin-dashboard">
  <div class="topbar">
    <div>
      <h1>🏗️ Pravin's Dashboard</h1>
      <p id="pravin-date"></p>
    </div>
    <div class="topbar-right">
      <button class="switch-user-btn" onclick="showUserSelect()">⇄ Switch User</button>
      <button class="cpw-topbar-btn" onclick="openCpwModal()">🔑 Password</button>
      <button class="cpw-topbar-btn" onclick="openEditUsername('pravin')">✏️ Name</button>
    </div>
  </div>

  <!-- Add/Edit Vendor Modal -->
  <div class="modal-overlay" id="vendor-modal" onclick="closeVendorModal(event)">
    <div class="modal">
      <div class="modal-header">
        <h2 id="vendor-modal-title">➕ Add Vendor</h2>
        <button class="modal-close" onclick="closeVendorModal()">✕</button>
      </div>
      <div style="display:flex;flex-direction:column;gap:14px">
        <div class="form-group"><label>Vendor Name *</label><input id="v-name" type="text" placeholder="e.g. ABC Suppliers"></div>
        <div class="form-group"><label>Contact Number *</label><input id="v-phone" type="tel" placeholder="e.g. 9876543210"></div>
        <div class="form-group"><label>Category / Notes</label><input id="v-notes" type="text" placeholder="e.g. Electrical, Plumbing..."></div>
        <button class="p-submit-btn" onclick="saveVendor()">💾 Save Vendor</button>
        <button class="p-submit-btn secondary" onclick="closeVendorModal()">Cancel</button>
      </div>
    </div>
  </div>

  <!-- Add/Edit Project Modal -->
  <div class="modal-overlay" id="project-modal" onclick="closeProjectModal(event)">
    <div class="modal">
      <div class="modal-header">
        <h2 id="project-modal-title">➕ Add Project</h2>
        <button class="modal-close" onclick="closeProjectModal()">✕</button>
      </div>
      <div style="display:flex;flex-direction:column;gap:14px">
        <div class="form-group"><label>Project Name *</label><input id="proj-name" type="text" placeholder="e.g. Villa Renovation"></div>
        <div class="form-group"><label>Location</label><input id="proj-location" type="text" placeholder="e.g. Bangalore"></div>
        <div class="form-group"><label>Contact Number</label><input id="proj-phone" type="tel" placeholder="Client contact number"></div>
        <div class="form-group"><label>Notes</label><input id="proj-notes" type="text" placeholder="Any details..."></div>
        <button class="p-submit-btn" onclick="saveProject()">💾 Save Project</button>
        <button class="p-submit-btn secondary" onclick="closeProjectModal()">Cancel</button>
      </div>
    </div>
  </div>

  <!-- Add Order Modal -->
  <div class="modal-overlay" id="order-modal" onclick="closeOrderModal(event)">
    <div class="modal">
      <div class="modal-header">
        <h2>📦 Add Material Order</h2>
        <button class="modal-close" onclick="closeOrderModal()">✕</button>
      </div>
      <div style="display:flex;flex-direction:column;gap:14px">
        <div class="form-group">
          <label>Project *</label>
          <select id="order-project">
            <option value="">-- Select Project --</option>
          </select>
        </div>
        <div class="form-group">
          <label>Vendor (to order from)</label>
          <select id="order-vendor">
            <option value="">-- Select Vendor --</option>
          </select>
        </div>
        <div class="form-group">
          <label>Order Heading *</label>
          <input id="order-heading" type="text" placeholder="e.g. Laminate Delivery, Hardware Order, Handle Procurement...">
          <div style="font-size:10px;color:#aaa;margin-top:4px;">💡 Tip: include 'Laminate', 'Hardware', or 'Handle' in the heading to auto-update work item milestones on delivery.</div>
        </div>
        <div class="form-group">
          <label>Materials List *</label>
          <div id="materials-container" style="display:flex;flex-direction:column;gap:8px;"></div>
          <button type="button" class="add-mat-btn" onclick="addMaterialRow()">+ Add Material</button>
        </div>
        <div class="form-group"><label>Order Notes</label><input id="order-notes" type="text" placeholder="Any special instructions..."></div>
        <button class="p-submit-btn" onclick="saveOrder()">📦 Place Order</button>
        <button class="p-submit-btn secondary" onclick="closeOrderModal()">Cancel</button>
      </div>
    </div>
  </div>

  <!-- Work Item Add/Edit Modal -->
  <div class="modal-overlay" id="wi-modal" onclick="closeWiModal(event)">
    <div class="modal">
      <div class="modal-header">
        <h2 id="wi-modal-title">📋 Add Work Item</h2>
        <button class="modal-close" onclick="closeWiModal()">✕</button>
      </div>
      <div style="display:flex;flex-direction:column;gap:14px">
        <div class="form-group"><label>Item Name *</label><input id="wi-name" type="text" placeholder="e.g. Ground Floor Vitrified Tile Flooring"></div>
        <div class="form-group"><label>Description</label><input id="wi-desc" type="text" placeholder="e.g. Providing and Laying Vitrified Tile Flooring..."></div>
        <div class="form-row">
          <div class="form-group">
            <label>Category *</label>
            <select id="wi-cat">
              <option value="Flooring">Flooring</option>
              <option value="Woodwork">Woodwork</option>
              <option value="Plumbing">Plumbing</option>
              <option value="Electrical">Electrical</option>
              <option value="Civil">Civil</option>
              <option value="Fire Fighting">Fire Fighting</option>
              <option value="HVAC">HVAC</option>
              <option value="Painting">Painting</option>
              <option value="Other">Other</option>
            </select>
          </div>
          <div class="form-group">
            <label>Project</label>
            <select id="wi-project"><option value="">-- No Project --</option></select>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>Status</label>
            <select id="wi-status">
              <option value="Draft">Draft</option>
              <option value="Confirmed">Confirmed</option>
            </select>
          </div>
          <div class="form-group">
            <label>Item Type</label>
            <select id="wi-type">
              <option value="Site Work">Site Work</option>
              <option value="Production">Production</option>
              <option value="Raw Material">Raw Material</option>
            </select>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>UOM (Unit)</label>
            <select id="wi-uom">
              <option value="SQM">SQM</option>
              <option value="SQFT">SQFT</option>
              <option value="RFT">RFT</option>
              <option value="NOS">NOS</option>
              <option value="KG">KG</option>
              <option value="MT">MT</option>
              <option value="LOT">LOT</option>
              <option value="LS">LS</option>
            </select>
          </div>
          <div class="form-group">
            <label>Quantity</label>
            <input id="wi-qty" type="number" min="0" placeholder="e.g. 1150">
          </div>
        </div>
        <div class="form-group">
          <label>Progress Mode</label>
          <select id="wi-prog-mode" onchange="toggleWiProgMode()">
            <option value="milestone">Milestone Steps (Recommended)</option>
            <option value="percent">Manual Percentage</option>
          </select>
        </div>
        <div id="wi-milestone-group" class="form-group">
          <label>Current Milestone</label>
          <select id="wi-milestone" class="wi-milestone-select">
            <option value="-1">Not Started</option>
            <option value="0">Site Masking</option>
            <option value="1">Material Procured</option>
            <option value="2">Preparing carcass</option>
            <option value="3">Laminate selection</option>
            <option value="4">Laminate Procured</option>
            <option value="5">Hardware procured</option>
            <option value="6">Outer laminate Pasting</option>
            <option value="7">Handle Selection</option>
            <option value="8">Handle Procured</option>
            <option value="9">Final finishing</option>
            <option value="10">Cleaning and Handover</option>
          </select>
        </div>
        <div id="wi-pct-group" class="form-group" style="display:none">
          <label>Completion % (0–100)</label>
          <input id="wi-pct" type="number" min="0" max="100" placeholder="e.g. 76">
        </div>
        <button class="p-submit-btn" onclick="saveWiItem()">💾 Save Work Item</button>
        <button class="p-submit-btn secondary" onclick="closeWiModal()">Cancel</button>
      </div>
    </div>
  </div>

  <div class="container">
    <!-- Stats -->
    <div class="p-stats">
      <div class="p-stat"><div class="p-stat-num" id="ps-projects">0</div><div class="p-stat-label">Projects</div></div>
      <div class="p-stat"><div class="p-stat-num" id="ps-vendors">0</div><div class="p-stat-label">Vendors</div></div>
      <div class="p-stat"><div class="p-stat-num" id="ps-pending">0</div><div class="p-stat-label">Orders Pending</div></div>
      <div class="p-stat"><div class="p-stat-num" id="ps-delivered">0</div><div class="p-stat-label">Delivered</div></div>
    </div>

    <!-- Tabs -->
    <div class="p-tabs">
      <button class="p-tab active" onclick="switchPravinTab('orders')">📦 Orders</button>
      <button class="p-tab" onclick="switchPravinTab('projects')">🏗️ Projects</button>
      <button class="p-tab" onclick="switchPravinTab('vendors')">🏪 Vendors</button>
      <button class="p-tab" onclick="switchPravinTab('workitems')">📋 Work Items</button>
      <button class="p-tab" onclick="switchPravinTab('status')">📊 Status</button>
      <button class="p-tab" onclick="switchPravinTab('report')">📈 Report</button>
    </div>

    <!-- ORDERS TAB -->
    <div id="ptab-orders">
      <div class="section-header">
        <h3>📦 Material Orders</h3>
        <button class="add-inline-btn" onclick="openOrderModal()">+ New Order</button>
      </div>
      <div style="margin-bottom:14px;display:flex;gap:8px;flex-wrap:wrap;align-items:center;">
        <select class="filter-select" id="order-filter-project" onchange="renderPravin()">
          <option value="">All Projects</option>
        </select>
        <select class="filter-select" id="order-filter-status" onchange="renderPravin()">
          <option value="">All Status</option>
          <option value="pending">Pending</option>
          <option value="ordered">Ordered</option>
          <option value="delivered">Delivered</option>
        </select>
      </div>
      <div class="card-grid" id="orders-list"></div>
    </div>

    <!-- PROJECTS TAB -->
    <div id="ptab-projects" style="display:none">
      <div class="section-header">
        <h3>🏗️ Projects</h3>
        <button class="add-inline-btn" onclick="openProjectModal()">+ New Project</button>
      </div>
      <div class="card-grid" id="projects-list"></div>
    </div>

    <!-- VENDORS TAB -->
    <div id="ptab-vendors" style="display:none">
      <div class="section-header">
        <h3>🏪 Vendors</h3>
        <button class="add-inline-btn" onclick="openVendorModal()">+ New Vendor</button>
      </div>
      <div class="card-grid" id="vendors-list"></div>
    </div>

    <!-- PROJECT STATUS TAB -->
    <div id="ptab-status" style="display:none">
      <div class="section-header" style="margin-bottom:12px;">
        <h3>📊 Project Status</h3>
      </div>
      <div id="project-status-list"></div>
    </div>

    <!-- WORK ITEMS TAB -->
    <div id="ptab-workitems" style="display:none">
      <div class="wi-section-header">
        <h3>📋 Work Items & Progress</h3>
        <button class="add-inline-btn" onclick="openWiModal()">+ New Work Item</button>
      </div>
      <div class="wi-stats">
        <div class="wi-stat"><div class="wi-stat-num" id="wi-s-total">0</div><div class="wi-stat-label">Total Items</div></div>
        <div class="wi-stat"><div class="wi-stat-num" id="wi-s-done" style="color:#166534">0</div><div class="wi-stat-label">Completed</div></div>
        <div class="wi-stat"><div class="wi-stat-num" id="wi-s-inprog" style="color:#2563eb">0</div><div class="wi-stat-label">In Progress</div></div>
        <div class="wi-stat"><div class="wi-stat-num" id="wi-s-notstart" style="color:#92400e">0</div><div class="wi-stat-label">Not Started</div></div>
      </div>
      <div class="wi-filter-row">
        <select class="filter-select" id="wi-filter-project" onchange="renderWorkItems()">
          <option value="">All Projects</option>
        </select>
        <select class="filter-select" id="wi-filter-cat" onchange="renderWorkItems()">
          <option value="">All Categories</option>
          <option value="Flooring">Flooring</option>
          <option value="Woodwork">Woodwork</option>
          <option value="Plumbing">Plumbing</option>
          <option value="Electrical">Electrical</option>
          <option value="Civil">Civil</option>
          <option value="Fire Fighting">Fire Fighting</option>
          <option value="HVAC">HVAC</option>
          <option value="Painting">Painting</option>
          <option value="Other">Other</option>
        </select>
        <select class="filter-select" id="wi-filter-type" onchange="renderWorkItems()">
          <option value="">All Types</option>
          <option value="Site Work">Site Work</option>
          <option value="Production">Production</option>
          <option value="Raw Material">Raw Material</option>
        </select>
      </div>
      <div id="work-items-list"></div>
    </div>

    <!-- MATERIALS REPORT TAB -->
    <div id="ptab-report" style="display:none">
      <div class="section-header" style="margin-bottom:12px;">
        <h3>📈 Materials Report</h3>
        <button class="report-export-btn btn-export-all" onclick="exportAllProjectsExcel()">⬇ Export All</button>
      </div>
      <div class="report-summary-bar" id="report-summary-bar"></div>
      <div id="report-projects-list"></div>
    </div>
  </div>
</div>
<!-- END PRAVIN -->

<!-- ══════════════ MANISH DASHBOARD ══════════════ -->
<div id="manish-dashboard">
  <div class="m-accent"></div>
  <div class="topbar">
    <div>
      <h1>✏️ <span id="manish-display-name">Manish</span>'s Dashboard</h1>
      <p id="manish-date"></p>
    </div>
    <div class="topbar-right">
      <button class="switch-user-btn" onclick="showUserSelect()">⇄ Switch</button>
      <button class="cpw-topbar-btn" onclick="openCpwModal()">🔑</button>
      <button class="cpw-topbar-btn" onclick="openEditUsername('manish')">✏️ Name</button>
    </div>
  </div>

  <!-- Add/Edit Project Modal -->
  <div class="m-proj-add-modal" id="m-proj-modal">
    <div class="m-modal-body">
      <div class="modal-header">
        <h2 id="m-modal-title">➕ Add Design Project</h2>
        <button class="modal-close" onclick="closeMProjectModal()">✕</button>
      </div>
      <div class="m-form-group"><label>Project Name *</label><input class="m-form-input" id="mp-name" placeholder="e.g. Villa Interior Design"></div>
      <div class="m-form-group"><label>Client / Location</label><input class="m-form-input" id="mp-client" placeholder="Client name or site location"></div>
      <div class="m-form-group"><label>Status</label>
        <select class="m-form-input" id="mp-status">
          <option value="pending">⏳ Pending</option>
          <option value="upcoming">📅 Upcoming</option>
          <option value="current">🔨 Current</option>
          <option value="onhold">⏸️ On Hold</option>
          <option value="completed">✅ Completed</option>
        </select>
      </div>
      <div class="m-form-group"><label>Start Date</label><input class="m-form-input" id="mp-start" type="date"></div>
      <div class="m-form-group"><label>Deadline</label><input class="m-form-input" id="mp-deadline" type="date"></div>
      <div class="m-form-group"><label>Notes</label><textarea class="m-form-input" id="mp-notes" rows="2" placeholder="Any notes..."></textarea></div>
      <button class="m-submit-btn" onclick="saveMProject()">💾 Save Project</button>
      <button class="m-submit-btn secondary" onclick="closeMProjectModal()">Cancel</button>
    </div>
  </div>

  <div class="container">
    <div class="m-stats">
      <div class="m-stat"><div class="m-stat-num" id="ms-total">0</div><div class="m-stat-label">Total</div></div>
      <div class="m-stat"><div class="m-stat-num" id="ms-current" style="color:#166534">0</div><div class="m-stat-label">Current</div></div>
      <div class="m-stat"><div class="m-stat-num" id="ms-pending" style="color:#92400e">0</div><div class="m-stat-label">Pending</div></div>
      <div class="m-stat"><div class="m-stat-num" id="ms-overdue" style="color:#dc2626">0</div><div class="m-stat-label">Overdue</div></div>
    </div>
    <div class="m-tabs">
      <button class="m-tab active" onclick="switchManishTab('all')">🗂 All</button>
      <button class="m-tab" onclick="switchManishTab('pending')">⏳ Pending</button>
      <button class="m-tab" onclick="switchManishTab('upcoming')">📅 Upcoming</button>
      <button class="m-tab" onclick="switchManishTab('current')">🔨 Current</button>
      <button class="m-tab" onclick="switchManishTab('completed')">✅ Done</button>
    </div>
    <div class="section-header" style="margin-bottom:12px;">
      <h3 id="manish-tab-title">🗂 All Projects</h3>
      <button class="add-inline-btn" style="background:#7c3aed;" onclick="openMProjectModal()">+ New Project</button>
    </div>
    <div id="manish-projects-list"></div>
  </div>
</div>
<!-- END MANISH -->

<!-- ══════════════ ADMIN DASHBOARD ══════════════ -->
<div id="admin-dashboard">
  <div class="admin-topbar-accent"></div>
  <div class="topbar">
    <div>
      <h1>🛡️ Admin Panel</h1>
      <p id="admin-date"></p>
    </div>
    <div class="topbar-right">
      <button class="switch-user-btn" onclick="showUserSelect()">⇄ Switch User</button>
      <button class="cpw-topbar-btn" onclick="openCpwModal()">🔑 Password</button>
      <button class="cpw-topbar-btn" onclick="openEditUsername('admin')">✏️ Name</button>
    </div>
  </div>
  <div class="container">
    <!-- Stats -->
    <div class="a-stats">
      <div class="a-stat"><div class="a-stat-num" id="as-total">0</div><div class="a-stat-label">Total Orders</div></div>
      <div class="a-stat"><div class="a-stat-num" id="as-pending">0</div><div class="a-stat-label">Awaiting Review</div></div>
      <div class="a-stat"><div class="a-stat-num" id="as-approved">0</div><div class="a-stat-label">Approved</div></div>
      <div class="a-stat"><div class="a-stat-num" id="as-denied">0</div><div class="a-stat-label">Denied</div></div>
    </div>

    <!-- Tabs -->
    <div class="a-tabs">
      <button class="a-tab active" onclick="switchAdminTab('review')">📋 Review Orders</button>
      <button class="a-tab" onclick="switchAdminTab('history')">📜 Decision History</button>
    </div>

    <!-- REVIEW TAB -->
    <div id="atab-review">
      <div class="admin-filter-bar">
        <select class="admin-filter-select" id="admin-filter-project" onchange="renderAdmin()">
          <option value="">All Projects</option>
        </select>
        <select class="admin-filter-select" id="admin-filter-approval" onchange="renderAdmin()">
          <option value="pending_review">⏳ Awaiting Review</option>
          <option value="">All Orders</option>
          <option value="approved">✅ Approved</option>
          <option value="denied">❌ Denied</option>
        </select>
      </div>
      <div id="admin-review-list"></div>
    </div>

    <!-- HISTORY TAB -->
    <div id="atab-history" style="display:none">
      <div id="admin-history-list"></div>
    </div>
  </div>
</div>
<!-- END ADMIN -->

<div class="success-toast" id="toast"></div>

<script>
// ══════════════════════════════════════════════════
//  PASSWORD SYSTEM
// ══════════════════════════════════════════════════
const DEFAULT_PASSWORD = '1234';
const PW_KEYS = { priyanka: 'pw_priyanka', pravin: 'pw_pravin', admin: 'pw_admin', manish: 'pw_manish' };
const USER_META = {
  priyanka: { avatar: '🌸', name: 'Priyanka', btnClass: 'priyanka-btn' },
  pravin:   { avatar: '🏗️', name: 'Pravin',   btnClass: 'pravin-btn' },
  admin:    { avatar: '🛡️', name: 'Admin',     btnClass: 'admin-btn' },
  manish:   { avatar: '✏️', name: 'Manish',    btnClass: 'manish-btn' }
};
let pendingUser = null;

function getPassword(user) {
  return localStorage.getItem(PW_KEYS[user]) || DEFAULT_PASSWORD;
}
function setPassword(user, pw) {
  localStorage.setItem(PW_KEYS[user], pw);
}

function showPasswordScreen(user) {
  pendingUser = user;
  const meta = USER_META[user];
  document.getElementById('user-select-screen').style.display = 'none';
  document.getElementById('pw-avatar').textContent = meta.avatar;
  document.getElementById('pw-name').textContent = meta.name;
  const submitBtn = document.getElementById('pw-submit');
  submitBtn.className = 'pw-submit ' + meta.btnClass;
  document.getElementById('pw-input').value = '';
  document.getElementById('pw-error').textContent = '';
  document.getElementById('pw-input').classList.remove('error');
  // Show hint only if password is still the default
  const isDefault = getPassword(user) === DEFAULT_PASSWORD;
  document.getElementById('pw-hint').style.display = isDefault ? 'block' : 'none';
  const screen = document.getElementById('password-screen');
  screen.style.display = 'flex';
  setTimeout(() => document.getElementById('pw-input').focus(), 100);
}

function submitPassword() {
  const entered = document.getElementById('pw-input').value;
  const correct = getPassword(pendingUser);
  if (entered === correct) {
    document.getElementById('password-screen').style.display = 'none';
    document.getElementById('pw-error').textContent = '';
    document.getElementById('pw-input').classList.remove('error');
    selectUser(pendingUser);
  } else {
    document.getElementById('pw-input').classList.add('error');
    const errEl = document.getElementById('pw-error');
    errEl.textContent = '❌ Incorrect password. Try again.';
    document.getElementById('pw-input').value = '';
    setTimeout(() => document.getElementById('pw-input').focus(), 50);
  }
}

function togglePwEye() {
  const inp = document.getElementById('pw-input');
  const eye = document.getElementById('pw-eye');
  if (inp.type === 'password') { inp.type = 'text'; eye.textContent = '🙈'; }
  else { inp.type = 'password'; eye.textContent = '👁️'; }
}

// ── Change Password Modal ──
function openCpwModal() {
  document.getElementById('cpw-current').value = '';
  document.getElementById('cpw-new').value = '';
  document.getElementById('cpw-confirm').value = '';
  document.getElementById('cpw-current-error').textContent = '';
  document.getElementById('cpw-error').textContent = '';
  document.getElementById('cpw-strength-bar').style.width = '0%';
  document.getElementById('cpw-strength-label').textContent = '';
  ['cpw-current','cpw-new','cpw-confirm'].forEach(id => document.getElementById(id).classList.remove('error'));
  // Style save button for current user
  if (pendingUser) {
    document.getElementById('cpw-save-btn').style.background = USER_META[pendingUser]?.btnClass === 'pravin-btn' ? '#2563eb' : USER_META[pendingUser]?.btnClass === 'admin-btn' ? '#d97706' : '#1a1a1a';
  }
  document.getElementById('cpw-modal').classList.add('open');
  setTimeout(() => document.getElementById('cpw-current').focus(), 100);
}

function closeCpwModal() {
  document.getElementById('cpw-modal').classList.remove('open');
}

function toggleCpwEye(inputId) {
  const inp = document.getElementById(inputId);
  inp.type = inp.type === 'password' ? 'text' : 'password';
}

function updateStrength() {
  const pw = document.getElementById('cpw-new').value;
  const bar = document.getElementById('cpw-strength-bar');
  const label = document.getElementById('cpw-strength-label');
  let score = 0;
  if (pw.length >= 4) score++;
  if (pw.length >= 8) score++;
  if (/[A-Z]/.test(pw) || /[0-9]/.test(pw)) score++;
  if (/[^a-zA-Z0-9]/.test(pw)) score++;
  const levels = [
    { w: '0%', bg: '#e0ddd6', text: '' },
    { w: '25%', bg: '#ef4444', text: 'Weak' },
    { w: '50%', bg: '#f97316', text: 'Fair' },
    { w: '75%', bg: '#eab308', text: 'Good' },
    { w: '100%', bg: '#22c55e', text: 'Strong' },
  ];
  const lvl = levels[score];
  bar.style.width = lvl.w; bar.style.background = lvl.bg;
  label.textContent = lvl.text; label.style.color = lvl.bg;
}

function savePassword() {
  const current = document.getElementById('cpw-current').value;
  const newPw   = document.getElementById('cpw-new').value;
  const confirm = document.getElementById('cpw-confirm').value;
  let hasError = false;

  document.getElementById('cpw-current-error').textContent = '';
  document.getElementById('cpw-error').textContent = '';
  ['cpw-current','cpw-new','cpw-confirm'].forEach(id => document.getElementById(id).classList.remove('error'));

  if (current !== getPassword(pendingUser)) {
    document.getElementById('cpw-current').classList.add('error');
    document.getElementById('cpw-current-error').textContent = '❌ Current password is incorrect.';
    hasError = true;
  }
  if (newPw.length < 4) {
    document.getElementById('cpw-new').classList.add('error');
    document.getElementById('cpw-error').textContent = '❌ New password must be at least 4 characters.';
    hasError = true;
  }
  if (!hasError && newPw !== confirm) {
    document.getElementById('cpw-confirm').classList.add('error');
    document.getElementById('cpw-error').textContent = '❌ Passwords do not match.';
    hasError = true;
  }
  if (hasError) return;

  setPassword(pendingUser, newPw);
  closeCpwModal();
  showToast('🔑 Password changed successfully!');
}

// ══════════════════════════════════════════════════
//  USER SELECTION
// ══════════════════════════════════════════════════
function selectUser(user) {
  ['priyanka-dashboard','pravin-dashboard','admin-dashboard','manish-dashboard','user-select-screen','password-screen'].forEach(id => {
    const el = document.getElementById(id); if (el) el.style.display = 'none';
  });
  if (user === 'priyanka') { document.getElementById('priyanka-dashboard').style.display = 'block'; initPriyanka(); }
  else if (user === 'pravin')  { document.getElementById('pravin-dashboard').style.display = 'block'; initPravin(); }
  else if (user === 'admin')   { document.getElementById('admin-dashboard').style.display = 'block'; initAdmin(); }
  else if (user === 'manish')  { document.getElementById('manish-dashboard').style.display = 'block'; initManish(); }
}
function showUserSelect() {
  ['priyanka-dashboard','pravin-dashboard','admin-dashboard','manish-dashboard','password-screen'].forEach(id => {
    const el = document.getElementById(id); if (el) el.style.display = 'none';
  });
  document.getElementById('user-select-screen').style.display = 'flex';
  refreshUsernameCards();
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.style.display = 'block';
  setTimeout(() => t.style.display = 'none', 2500);
}

// ══════════════════════════════════════════════════
//  PRIYANKA'S ERP DASHBOARD
// ══════════════════════════════════════════════════
const TODAY = new Date().toISOString().slice(0,10);
let items = [];
let activeTab = 'today';
let typeFilter = 'all';
let selectedType = 'meeting';
let editingType = 'meeting';
let editingId = null;
let alertItemId = null;
let notifiedIds = [];
let searchQuery = { today: '', all: '' };
let snoozedIds = [];
let dismissedToday = {};
let dockCollapsed = false;

function initPriyanka() {
  items = JSON.parse(localStorage.getItem('mam_erp') || '[]');
  notifiedIds = JSON.parse(localStorage.getItem('mam_notified') || '[]');
  snoozedIds = JSON.parse(sessionStorage.getItem('mam_snoozed') || '[]');
  dismissedToday = JSON.parse(localStorage.getItem('mam_dismissed_today') || '{}');

  if (!items.length) {
    const now = new Date();
    const in10 = new Date(now.getTime() + 10*60000);
    const hh = String(in10.getHours()).padStart(2,'0');
    const mm = String(in10.getMinutes()).padStart(2,'0');
    items = [
      {id:1, type:'meeting', title:'Doctor checkup', date:TODAY, time:'10:00', note:'General health checkup', done:false, remindBefore:10},
      {id:2, type:'task', title:'Pay electricity bill', date:TODAY, time:'14:00', note:'Due today', done:false, remindBefore:10},
      {id:3, type:'reminder', title:'Demo alert — fires in 10 min', date:TODAY, time:`${hh}:${mm}`, note:'This is a test reminder', done:false, remindBefore:10},
      {id:4, type:'meeting', title:'Family gathering', date:'2026-05-30', time:'18:00', note:"At uncle's place", done:false, remindBefore:30},
    ];
    save();
  }

  document.getElementById('today-date').textContent = new Date().toLocaleDateString('en-IN',{weekday:'long',year:'numeric',month:'long',day:'numeric'});
  document.getElementById('new-date').value = TODAY;
  activeTab = 'today';
  switchTab('today');
  updateNotifBtn();
  renderPersistDock();
  checkReminders();
  setInterval(checkReminders, 15000);
  setInterval(renderPersistDock, 30000);
}

function save() { localStorage.setItem('mam_erp', JSON.stringify(items)); renderPersistDock(); }

function updateNotifBtn() {
  if (typeof Notification === 'undefined') return;
  const btn = document.getElementById('notif-btn');
  const p = Notification.permission;
  if (p === 'granted') { btn.textContent = '🔔 Alerts ON'; btn.className = 'notif-status on'; }
  else if (p === 'denied') { btn.textContent = '🔕 Blocked'; btn.className = 'notif-status blocked'; }
  else { btn.textContent = '🔔 Enable Alerts'; btn.className = 'notif-status off'; }
}
function requestNotifPermission() {
  if (Notification.permission === 'denied') { alert('Notifications blocked. Allow in browser settings.'); return; }
  Notification.requestPermission().then(updateNotifBtn);
}
function fireNotification(item) {
  if (Notification.permission !== 'granted') return;
  const icon = item.type==='meeting'?'📅':item.type==='task'?'✅':'🔔';
  const n = new Notification(`${icon} ${item.title}`, { body: item.note||'Time to act!', requireInteraction: true });
  n.onclick = () => window.focus();
}
function checkReminders() {
  if (!document.getElementById('priyanka-dashboard') || document.getElementById('priyanka-dashboard').style.display === 'none') return;
  const now = new Date();
  items.filter(i => !i.done && i.date && i.time).forEach(item => {
    const fireAt = new Date(item.date+'T'+item.time) - (item.remindBefore||0)*60000;
    const key = item.id+'_'+item.date;
    if (now >= fireAt && now < fireAt + 60000 && !notifiedIds.includes(key)) {
      notifiedIds.push(key);
      localStorage.setItem('mam_notified', JSON.stringify(notifiedIds));
      showAlertBanner(item);
      fireNotification(item);
    }
  });
}

let alertBannerTimer = null;
function showAlertBanner(item) {
  alertItemId = item.id;
  document.getElementById('alert-title').textContent = item.title;
  document.getElementById('alert-sub').textContent = [item.note, item.time].filter(Boolean).join(' · ');
  const b = document.getElementById('alert-banner');
  b.style.display = 'block';
  if (alertBannerTimer) clearTimeout(alertBannerTimer);
  alertBannerTimer = setTimeout(() => b.style.display='none', 30000);
}
function dismissAlert() { document.getElementById('alert-banner').style.display='none'; }
function doneFromAlert() { if(alertItemId) toggleDone(alertItemId); dismissAlert(); }

function toggleNewRepeat(cb) { document.getElementById('new-repeat-fields').classList.toggle('show', cb.checked); }
function toggleEditRepeat(cb) { document.getElementById('edit-repeat-fields').classList.toggle('show', cb.checked); }
function setRepeatEndQuick(days) { const d = new Date(); d.setDate(d.getDate()+days); document.getElementById('new-repeat-end').value = d.toISOString().slice(0,10); }

function openEditModal(id) {
  const item = items.find(i => i.id===id);
  if (!item) return;
  editingId = id; editingType = item.type;
  document.getElementById('edit-title').value = item.title;
  document.getElementById('edit-date').value = item.date || TODAY;
  document.getElementById('edit-time').value = item.time || '';
  document.getElementById('edit-note').value = item.note || '';
  document.getElementById('edit-remind').value = item.remindBefore ?? 10;
  const hasRepeat = !!(item.repeatEnd);
  document.getElementById('edit-repeat-on').checked = hasRepeat;
  document.getElementById('edit-repeat-fields').classList.toggle('show', hasRepeat);
  document.getElementById('edit-repeat-end').value = item.repeatEnd || '';
  document.getElementById('edit-repeat-freq').value = item.repeatFreq || 'daily';
  selectEditType(item.type);
  document.getElementById('edit-modal').classList.add('open');
}
function closeEditModal(e) {
  if (e && e.target !== document.getElementById('edit-modal')) return;
  document.getElementById('edit-modal').classList.remove('open');
  editingId = null;
}
function selectEditType(type) {
  editingType = type;
  ['meeting','task','reminder'].forEach(t => {
    document.getElementById('eopt-'+t).className = 'type-opt' + (t===type ? ' selected-'+type : '');
  });
}
function setEditQuickDate(days) { const d = new Date(); d.setDate(d.getDate()+days); document.getElementById('edit-date').value = d.toISOString().slice(0,10); }
function saveEdit() {
  const title = document.getElementById('edit-title').value.trim();
  if (!title) { alert('Please enter a title!'); return; }
  const item = items.find(i => i.id===editingId);
  if (!item) return;
  item.type = editingType; item.title = title;
  item.date = document.getElementById('edit-date').value || TODAY;
  item.time = document.getElementById('edit-time').value;
  item.note = document.getElementById('edit-note').value.trim();
  item.remindBefore = parseInt(document.getElementById('edit-remind').value);
  if (document.getElementById('edit-repeat-on').checked) {
    item.repeatEnd = document.getElementById('edit-repeat-end').value;
    item.repeatFreq = document.getElementById('edit-repeat-freq').value;
  } else { delete item.repeatEnd; delete item.repeatFreq; }
  const key = item.id+'_'+item.date;
  notifiedIds = notifiedIds.filter(k => k !== key);
  localStorage.setItem('mam_notified', JSON.stringify(notifiedIds));
  save(); document.getElementById('edit-modal').classList.remove('open'); editingId = null;
  showToast('✓ Changes saved!'); render();
}

function selectType(type) {
  selectedType = type;
  ['meeting','task','reminder'].forEach(t => {
    document.getElementById('opt-'+t).className = 'type-opt' + (t===type ? ' selected-'+type : '');
  });
}
function setQuickDate(days) { const d = new Date(); d.setDate(d.getDate()+days); document.getElementById('new-date').value = d.toISOString().slice(0,10); }

function onSearch(tab) {
  const input = document.getElementById('search-'+tab);
  searchQuery[tab] = input.value.trim().toLowerCase();
  const clearBtn = document.getElementById('search-clear-'+tab);
  clearBtn.classList.toggle('visible', searchQuery[tab].length > 0);
  render();
}
function clearSearch(tab) {
  document.getElementById('search-'+tab).value = '';
  searchQuery[tab] = '';
  document.getElementById('search-clear-'+tab).classList.remove('visible');
  render();
}
function matchesSearch(item, q) {
  if (!q) return true;
  return (item.title||'').toLowerCase().includes(q)||(item.note||'').toLowerCase().includes(q)||(item.type||'').toLowerCase().includes(q)||(item.date||'').includes(q);
}

function minutesUntil(dateStr, timeStr) { if (!timeStr) return null; return Math.round((new Date(dateStr+'T'+timeStr)-new Date())/60000); }
function timeStr2(date, time) {
  if (!date) return '';
  const d = new Date(date+'T00:00:00');
  const dayStr = date===TODAY?'Today':d.toLocaleDateString('en-IN',{day:'numeric',month:'short'});
  return time ? `${dayStr} · ${time}` : dayStr;
}
function renderItem(item) {
  const mins = item.date===TODAY ? minutesUntil(item.date, item.time) : null;
  const dueSoon = mins!==null && mins>=0 && mins<=15;
  const overdue = mins!==null && mins<0 && !item.done;
  const soonTag = dueSoon&&!item.done ? `<span class="due-soon-tag">${mins===0?'NOW':mins+'m'}</span>` : (overdue&&!item.done ? '<span class="due-soon-tag">OVERDUE</span>' : '');
  const repeatTag = item.repeatEnd ? `<span class="badge badge-repeat">🔁 repeating</span>` : '';
  const div = document.createElement('div');
  div.className = 'item'+(item.done?' done':'')+(((dueSoon||overdue)&&!item.done)?' due-soon':'')+(item.repeatEnd&&!item.done?' repeat-item':'');
  div.innerHTML = `
    <button class="done-btn${item.done?' checked':''}" onclick="toggleDone(${item.id})" title="Mark done">${item.done?'✓':''}</button>
    <div class="item-body">
      <div class="item-title">${item.title}<span class="badge badge-${item.type}">${item.type}</span>${repeatTag}${soonTag}</div>
      <div class="item-meta">${timeStr2(item.date,item.time)}${item.repeatEnd?' → '+item.repeatEnd:''}${item.note?' · '+item.note:''}</div>
    </div>
    <div class="item-actions">
      <button class="icon-btn edit" onclick="openEditModal(${item.id})" title="Edit">✏️</button>
      <button class="icon-btn del" onclick="deleteItem(${item.id})" title="Delete">✕</button>
    </div>`;
  return div;
}
function renderEmpty(msg, icon) {
  const div = document.createElement('div');
  div.className = 'empty';
  div.innerHTML = `<div class="empty-icon">${icon||'🎉'}</div>${msg}`;
  return div;
}
function isActiveToday(item) {
  if (item.done) return false;
  if (item.repeatEnd) return item.date <= TODAY && item.repeatEnd >= TODAY;
  return item.date === TODAY;
}
function render() {
  const pending = items.filter(i=>!i.done);
  const todayAll = items.filter(i=>!i.done && isActiveToday(i));
  const filtered = typeFilter==='all'?todayAll:todayAll.filter(i=>i.type===typeFilter);
  document.getElementById('s-pending').textContent = pending.length;
  document.getElementById('s-today').textContent = todayAll.length;
  document.getElementById('s-done').textContent = items.filter(i=>i.done).length;

  const qToday = searchQuery.today||'';
  const filteredSearch = filtered.filter(i=>matchesSearch(i,qToday));
  const lt = document.getElementById('list-today'); lt.innerHTML='';
  const countToday = document.getElementById('search-count-today');
  if (qToday) { countToday.textContent=`${filteredSearch.length} result${filteredSearch.length!==1?'s':''} for "${qToday}"`; countToday.classList.add('visible'); } else countToday.classList.remove('visible');
  if (!filteredSearch.length) lt.appendChild(renderEmpty(qToday?`No items match "${qToday}"`:'Nothing for today — all clear! 🌟', qToday?'🔍':'🌸'));
  else filteredSearch.sort((a,b)=>(a.time||'23:59').localeCompare(b.time||'23:59')).forEach(i=>lt.appendChild(renderItem(i)));

  const qAll = searchQuery.all||'';
  const allPending = [...pending].sort((a,b)=>a.date.localeCompare(b.date)||(a.time||'23:59').localeCompare(b.time||'23:59'));
  const allFiltered = allPending.filter(i=>matchesSearch(i,qAll));
  const la = document.getElementById('list-all'); la.innerHTML='';
  const countAll = document.getElementById('search-count-all');
  if (qAll) { countAll.textContent=`${allFiltered.length} result${allFiltered.length!==1?'s':''} for "${qAll}"`; countAll.classList.add('visible'); } else countAll.classList.remove('visible');
  if (!allFiltered.length) la.appendChild(renderEmpty(qAll?`No items match "${qAll}"`:'Everything is done!', qAll?'🔍':'🎉'));
  else allFiltered.forEach(i=>la.appendChild(renderItem(i)));

  const ld = document.getElementById('list-done'); ld.innerHTML='';
  const done = items.filter(i=>i.done);
  if (!done.length) ld.appendChild(renderEmpty('No completed items yet.','⏳'));
  else done.forEach(i=>ld.appendChild(renderItem(i)));
}
function switchTab(tab) {
  activeTab = tab;
  ['today','all','done','add'].forEach(t=>{ document.getElementById('tab-'+t).style.display=t===tab?'':'none'; });
  document.querySelectorAll('#priyanka-dashboard .tab').forEach((b,i)=>{ b.classList.toggle('active',['today','all','done','add'][i]===tab); });
  if (tab!=='add') render();
}
function setFilter(f,btn) {
  typeFilter=f;
  document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  render();
}
function toggleDone(id) { const item=items.find(i=>i.id===id); if(item){item.done=!item.done; save(); render();} }
function deleteItem(id) { if(confirm('Delete this item?')){items=items.filter(i=>i.id!==id); save(); render();} }
function clearDone() { if(confirm('Clear all completed items?')){items=items.filter(i=>!i.done); save(); render();} }

function addItem() {
  const title=document.getElementById('new-title').value.trim();
  if (!title) { alert('Please enter a title!'); return; }
  const repeatOn = document.getElementById('new-repeat-on').checked;
  const repeatEnd = repeatOn ? document.getElementById('new-repeat-end').value : null;
  const repeatFreq = repeatOn ? document.getElementById('new-repeat-freq').value : null;
  if (repeatOn && !repeatEnd) { alert('Please set a "Repeat Until" date!'); return; }
  const newItem = {
    id:Date.now(), type:selectedType, title,
    date:document.getElementById('new-date').value||TODAY,
    time:document.getElementById('new-time').value,
    note:document.getElementById('new-note').value.trim(),
    remindBefore:parseInt(document.getElementById('new-remind').value),
    done:false
  };
  if (repeatOn) { newItem.repeatEnd=repeatEnd; newItem.repeatFreq=repeatFreq; }
  items.push(newItem); save();
  document.getElementById('new-title').value='';
  document.getElementById('new-note').value='';
  document.getElementById('new-time').value='';
  document.getElementById('new-date').value=TODAY;
  document.getElementById('new-repeat-on').checked=false;
  document.getElementById('new-repeat-fields').classList.remove('show');
  showToast('✓ Added successfully!');
  switchTab('today');
}

// Persistent dock
function getTodayStr() { return new Date().toISOString().slice(0,10); }
function cleanDismissed() {
  const t=getTodayStr();
  Object.keys(dismissedToday).forEach(k=>{ if(dismissedToday[k]!==t) delete dismissedToday[k]; });
  localStorage.setItem('mam_dismissed_today', JSON.stringify(dismissedToday));
}
function isPersistActive(item) {
  if (item.done) return false;
  const today=getTodayStr(), now=new Date();
  if (item.repeatEnd) {
    if (item.date>today||item.repeatEnd<today) return false;
    if (item.time) { const todayTime=new Date(today+'T'+item.time); if(now<todayTime) return false; }
    return true;
  }
  if (item.date>today) return false;
  if (item.date===today&&item.time) { const idt=new Date(today+'T'+item.time); if(now<idt) return false; }
  return item.date<=today;
}
function getPersistentItems() {
  cleanDismissed();
  const today=getTodayStr(), nextWeek=new Date();
  nextWeek.setDate(nextWeek.getDate()+7);
  const weekStr=nextWeek.toISOString().slice(0,10);
  return items.filter(item=>{
    if(!isPersistActive(item)) return false;
    if(dismissedToday[item.id]===today) return false;
    if(snoozedIds.includes(item.id)) return false;
    if(item.repeatEnd) return true;
    return item.date<=weekStr;
  }).sort((a,b)=>{ const aT=a.date===today?0:1, bT=b.date===today?0:1; return aT-bT||(a.time||'23:59').localeCompare(b.time||'23:59'); });
}
function getUrgency(item) {
  const today=getTodayStr();
  if (item.repeatEnd) return {level:'repeat',label:`🔁 Repeating until ${item.repeatEnd}`};
  const tomorrow=new Date(); tomorrow.setDate(tomorrow.getDate()+1);
  const tmrStr=tomorrow.toISOString().slice(0,10);
  if (item.date===today) {
    if (item.time) {
      const mins=Math.round((new Date(item.date+'T'+item.time)-new Date())/60000);
      if(mins<0) return {level:'urgent',label:`⚠️ Overdue by ${Math.abs(mins)}m`};
      if(mins<=60) return {level:'urgent',label:`⚡ Due in ${mins}m`};
    }
    return {level:'today',label:'📅 Due today'};
  }
  if(item.date<today) return {level:'urgent',label:'⚠️ Overdue'};
  if(item.date===tmrStr) return {level:'today',label:'🌅 Due tomorrow'};
  return {level:'week',label:'📆 Due this week'};
}
function renderPersistDock() {
  if (!document.getElementById('priyanka-dashboard')||document.getElementById('priyanka-dashboard').style.display==='none') return;
  const dock=document.getElementById('persist-dock');
  const toggle=document.getElementById('persist-toggle');
  const countEl=document.getElementById('persist-count');
  const persistItems=getPersistentItems();
  countEl.textContent=persistItems.length;
  if(!persistItems.length){dock.innerHTML='';toggle.style.display='none';return;}
  if(dockCollapsed){dock.innerHTML='';toggle.style.display='flex';return;}
  toggle.style.display='none'; dock.innerHTML='';
  persistItems.slice(0,3).forEach(item=>{
    const urg=getUrgency(item);
    const icon=item.type==='meeting'?'📅':item.type==='task'?'✅':'🔔';
    const timePart=item.time?` · ${item.time}`:'';
    const notePart=item.note?` · ${item.note}`:'';
    const cardClass=item.repeatEnd?'repeat-card':urg.level;
    const card=document.createElement('div');
    card.className=`persist-card ${cardClass}`; card.id='pcard-'+item.id;
    card.innerHTML=`<button class="persist-close" onclick="dismissPersist(${item.id})">✕</button>
      <div class="persist-card-top"><span class="persist-icon">${icon}</span>
        <div class="persist-body">
          <div class="persist-title">${item.title}</div>
          <div class="persist-sub">${item.date===getTodayStr()?'Today':new Date(item.date+'T00:00:00').toLocaleDateString('en-IN',{day:'numeric',month:'short'})}${timePart}${notePart}</div>
          <span class="persist-urgency u-${urg.level}">${urg.label}</span>
        </div>
      </div>
      <div class="persist-actions">
        <button class="persist-btn snooze" onclick="snoozePersist(${item.id})">😴 Snooze</button>
        <button class="persist-btn done-p" onclick="donePersist(${item.id})">✓ Done</button>
      </div>`;
    dock.appendChild(card);
  });
  if(persistItems.length>3){
    const more=document.createElement('div');
    more.style.cssText='text-align:center;font-size:11px;color:#888;background:#fff;border-radius:10px;padding:6px 12px;border:1px solid #e8e6e0;cursor:pointer;pointer-events:all;';
    more.textContent=`+${persistItems.length-3} more pending`;
    more.onclick=()=>switchTab('all');
    dock.appendChild(more);
  }
}
function dismissPersist(id){dismissedToday[id]=getTodayStr();localStorage.setItem('mam_dismissed_today',JSON.stringify(dismissedToday));renderPersistDock();}
function snoozePersist(id){snoozedIds.push(id);sessionStorage.setItem('mam_snoozed',JSON.stringify(snoozedIds));renderPersistDock();showToast('😴 Snoozed!');}
function donePersist(id){toggleDone(id);renderPersistDock();}
function collapseDock(){dockCollapsed=true;renderPersistDock();}
function expandDock(){dockCollapsed=false;renderPersistDock();}

// ══════════════════════════════════════════════════
//  PRAVIN'S DASHBOARD
// ══════════════════════════════════════════════════
let pVendors = [];
let pProjects = [];
let pOrders = [];
let pWorkItems = [];
let editingVendorId = null;
let editingProjectId = null;
let activePravinTab = 'orders';

function initPravin() {
  pVendors = JSON.parse(localStorage.getItem('pravin_vendors') || '[]');
  pProjects = JSON.parse(localStorage.getItem('pravin_projects') || '[]');
  pOrders = JSON.parse(localStorage.getItem('pravin_orders') || '[]');
  pWorkItems = JSON.parse(localStorage.getItem('pravin_workitems') || '[]');
  document.getElementById('pravin-date').textContent = new Date().toLocaleDateString('en-IN',{weekday:'long',year:'numeric',month:'long',day:'numeric'});
  renderPravin();
}

function savePravin() {
  localStorage.setItem('pravin_vendors', JSON.stringify(pVendors));
  localStorage.setItem('pravin_projects', JSON.stringify(pProjects));
  localStorage.setItem('pravin_orders', JSON.stringify(pOrders));
  localStorage.setItem('pravin_workitems', JSON.stringify(pWorkItems));
}

function switchPravinTab(tab) {
  activePravinTab = tab;
  ['orders','projects','vendors','workitems','status','report'].forEach(t => {
    document.getElementById('ptab-'+t).style.display = t===tab ? '' : 'none';
  });
  document.querySelectorAll('.p-tab').forEach((b,i) => {
    b.classList.toggle('active', ['orders','projects','vendors','workitems','status','report'][i]===tab);
  });
  renderPravin();
}

function renderPravin() {
  pVendors = JSON.parse(localStorage.getItem('pravin_vendors') || '[]');
  pProjects = JSON.parse(localStorage.getItem('pravin_projects') || '[]');
  pOrders = JSON.parse(localStorage.getItem('pravin_orders') || '[]');
  document.getElementById('ps-projects').textContent = pProjects.length;
  document.getElementById('ps-vendors').textContent = pVendors.length;
  document.getElementById('ps-pending').textContent = pOrders.filter(o=>o.status==='pending'||o.status==='ordered').length;
  document.getElementById('ps-delivered').textContent = pOrders.filter(o=>o.status==='delivered').length;
  renderProjects();
  renderVendors();
  renderOrders();
  populateOrderDropdowns();
  if (activePravinTab === 'status') renderProjectStatus();
  if (activePravinTab === 'report') renderMaterialsReport();
  if (activePravinTab === 'workitems') renderWorkItems();
}

// ── VENDORS ──
function openVendorModal(id) {
  editingVendorId = id || null;
  const v = id ? pVendors.find(v=>v.id===id) : null;
  document.getElementById('vendor-modal-title').textContent = id ? '✏️ Edit Vendor' : '➕ Add Vendor';
  document.getElementById('v-name').value = v ? v.name : '';
  document.getElementById('v-phone').value = v ? v.phone : '';
  document.getElementById('v-notes').value = v ? v.notes : '';
  document.getElementById('vendor-modal').classList.add('open');
}
function closeVendorModal(e) {
  if (e && e.target !== document.getElementById('vendor-modal')) return;
  document.getElementById('vendor-modal').classList.remove('open');
  editingVendorId = null;
}
function saveVendor() {
  const name = document.getElementById('v-name').value.trim();
  const phone = document.getElementById('v-phone').value.trim();
  if (!name) { alert('Vendor name is required!'); return; }
  if (!phone) { alert('Contact number is required!'); return; }
  if (editingVendorId) {
    const v = pVendors.find(v=>v.id===editingVendorId);
    if (v) { v.name=name; v.phone=phone; v.notes=document.getElementById('v-notes').value.trim(); }
  } else {
    pVendors.push({id:Date.now(), name, phone, notes:document.getElementById('v-notes').value.trim()});
  }
  savePravin(); document.getElementById('vendor-modal').classList.remove('open');
  showToast(editingVendorId ? '✓ Vendor updated!' : '✓ Vendor added!');
  editingVendorId = null; renderPravin();
}
function deleteVendor(id) {
  if (confirm('Delete this vendor?')) { pVendors=pVendors.filter(v=>v.id!==id); savePravin(); renderPravin(); }
}
function renderVendors() {
  const el = document.getElementById('vendors-list'); el.innerHTML='';
  if (!pVendors.length) { el.innerHTML='<div class="empty"><div class="empty-icon">🏪</div>No vendors yet. Add your first vendor!</div>'; return; }
  pVendors.forEach(v => {
    const d = document.createElement('div');
    d.className = 'p-card';
    d.innerHTML = `<span class="p-card-icon">🏪</span>
      <div class="p-card-body">
        <div class="p-card-name">${v.name}</div>
        <div class="p-card-sub">📞 ${v.phone}${v.notes?' · '+v.notes:''}</div>
      </div>
      <div class="p-card-actions">
        <button class="icon-btn edit" onclick="openVendorModal(${v.id})" title="Edit">✏️</button>
        <button class="icon-btn del" onclick="deleteVendor(${v.id})" title="Delete">✕</button>
      </div>`;
    el.appendChild(d);
  });
}

// ── PROJECTS ──
function openProjectModal(id) {
  editingProjectId = id || null;
  const p = id ? pProjects.find(p=>p.id===id) : null;
  document.getElementById('project-modal-title').textContent = id ? '✏️ Edit Project' : '➕ Add Project';
  document.getElementById('proj-name').value = p ? p.name : '';
  document.getElementById('proj-location').value = p ? p.location : '';
  document.getElementById('proj-phone').value = p ? p.phone : '';
  document.getElementById('proj-notes').value = p ? p.notes : '';
  document.getElementById('project-modal').classList.add('open');
}
function closeProjectModal(e) {
  if (e && e.target !== document.getElementById('project-modal')) return;
  document.getElementById('project-modal').classList.remove('open');
  editingProjectId = null;
}
function saveProject() {
  const name = document.getElementById('proj-name').value.trim();
  if (!name) { alert('Project name is required!'); return; }
  if (editingProjectId) {
    const p = pProjects.find(p=>p.id===editingProjectId);
    if (p) { p.name=name; p.location=document.getElementById('proj-location').value.trim(); p.phone=document.getElementById('proj-phone').value.trim(); p.notes=document.getElementById('proj-notes').value.trim(); }
  } else {
    pProjects.push({id:Date.now(), name, location:document.getElementById('proj-location').value.trim(), phone:document.getElementById('proj-phone').value.trim(), notes:document.getElementById('proj-notes').value.trim()});
  }
  savePravin(); document.getElementById('project-modal').classList.remove('open');
  showToast(editingProjectId ? '✓ Project updated!' : '✓ Project added!');
  editingProjectId = null; renderPravin();
}
function deleteProject(id) {
  if (confirm('Delete this project? Related orders will remain.')) { pProjects=pProjects.filter(p=>p.id!==id); savePravin(); renderPravin(); }
}
function renderProjects() {
  const el = document.getElementById('projects-list'); el.innerHTML='';
  if (!pProjects.length) { el.innerHTML='<div class="empty"><div class="empty-icon">🏗️</div>No projects yet. Add your first project!</div>'; return; }
  pProjects.forEach(p => {
    const orderCount = pOrders.filter(o=>o.projectId===p.id).length;
    const d = document.createElement('div');
    d.className = 'p-card';
    d.innerHTML = `<span class="p-card-icon">🏗️</span>
      <div class="p-card-body">
        <div class="p-card-name">${p.name}</div>
        <div class="p-card-sub">${p.location?'📍 '+p.location+' · ':''}${p.phone?'📞 '+p.phone+' · ':''}${orderCount} order${orderCount!==1?'s':''}${p.notes?' · '+p.notes:''}</div>
      </div>
      <div class="p-card-actions">
        <button class="icon-btn edit" onclick="openProjectModal(${p.id})" title="Edit">✏️</button>
        <button class="icon-btn del" onclick="deleteProject(${p.id})" title="Delete">✕</button>
      </div>`;
    el.appendChild(d);
  });
}

// ── ORDERS ──
function populateOrderDropdowns() {
  // For add-order modal
  const ps = document.getElementById('order-project');
  const vs = document.getElementById('order-vendor');
  if (ps) {
    const curProj = ps.value;
    ps.innerHTML = '<option value="">-- Select Project --</option>';
    pProjects.forEach(p => { const o=document.createElement('option'); o.value=p.id; o.textContent=p.name; ps.appendChild(o); });
    if (curProj) ps.value = curProj;
  }
  if (vs) {
    const curVend = vs.value;
    vs.innerHTML = '<option value="">-- Select Vendor --</option>';
    pVendors.forEach(v => { const o=document.createElement('option'); o.value=v.id; o.textContent=`${v.name} (${v.phone})`; vs.appendChild(o); });
    if (curVend) vs.value = curVend;
  }

  // For filter dropdowns
  const fp = document.getElementById('order-filter-project');
  if (fp) {
    const cur = fp.value;
    fp.innerHTML = '<option value="">All Projects</option>';
    pProjects.forEach(p => { const o=document.createElement('option'); o.value=p.id; o.textContent=p.name; fp.appendChild(o); });
    if (cur) fp.value = cur;
  }
}

// ══════════════════════════════════════════════════
//  ADMIN DASHBOARD
// ══════════════════════════════════════════════════
let activeAdminTab = 'review';
let adminCollapsed = {};

function initAdmin() {
  // Load Pravin's data (shared localStorage)
  pVendors = JSON.parse(localStorage.getItem('pravin_vendors') || '[]');
  pProjects = JSON.parse(localStorage.getItem('pravin_projects') || '[]');
  pOrders = JSON.parse(localStorage.getItem('pravin_orders') || '[]');
  document.getElementById('admin-date').textContent = new Date().toLocaleDateString('en-IN',{weekday:'long',year:'numeric',month:'long',day:'numeric'});
  populateAdminProjectFilter();
  renderAdmin();
}

function populateAdminProjectFilter() {
  const fp = document.getElementById('admin-filter-project');
  if (!fp) return;
  const cur = fp.value;
  fp.innerHTML = '<option value="">All Projects</option>';
  pProjects.forEach(p => {
    const o = document.createElement('option'); o.value = p.id; o.textContent = p.name; fp.appendChild(o);
  });
  if (cur) fp.value = cur;
}

function switchAdminTab(tab) {
  activeAdminTab = tab;
  ['review','history'].forEach(t => {
    document.getElementById('atab-'+t).style.display = t===tab ? '' : 'none';
  });
  document.querySelectorAll('.a-tab').forEach((b,i) => {
    b.classList.toggle('active', ['review','history'][i]===tab);
  });
  renderAdmin();
}

function adminApprove(orderId) {
  // Reload fresh data in case Pravin made changes
  pOrders = JSON.parse(localStorage.getItem('pravin_orders') || '[]');
  const o = pOrders.find(o => o.id === orderId);
  if (!o) return;
  o.adminStatus = 'approved';
  o.adminDecidedAt = new Date().toLocaleDateString('en-IN');
  o.adminReason = '';
  savePravin();
  showToast('✅ Order Approved!');
  renderAdmin();
}

function adminDeny(orderId) {
  pOrders = JSON.parse(localStorage.getItem('pravin_orders') || '[]');
  const o = pOrders.find(o => o.id === orderId);
  if (!o) return;
  const reasonEl = document.getElementById('deny-reason-'+orderId);
  const reason = reasonEl ? reasonEl.value.trim() : '';
  o.adminStatus = 'denied';
  o.adminDecidedAt = new Date().toLocaleDateString('en-IN');
  o.adminReason = reason;
  savePravin();
  showToast('❌ Order Denied.');
  renderAdmin();
}

function adminReset(orderId) {
  pOrders = JSON.parse(localStorage.getItem('pravin_orders') || '[]');
  const o = pOrders.find(o => o.id === orderId);
  if (!o) return;
  delete o.adminStatus; delete o.adminDecidedAt; delete o.adminReason;
  savePravin();
  showToast('↩️ Decision reset.');
  renderAdmin();
}

function toggleAdminProject(projId) {
  adminCollapsed[projId] = !adminCollapsed[projId];
  renderAdmin();
}

function renderAdmin() {
  pVendors = JSON.parse(localStorage.getItem('pravin_vendors') || '[]');
  pProjects = JSON.parse(localStorage.getItem('pravin_projects') || '[]');
  pOrders = JSON.parse(localStorage.getItem('pravin_orders') || '[]');
  populateAdminProjectFilter();

  const filterProject = document.getElementById('admin-filter-project')?.value;
  const filterApproval = document.getElementById('admin-filter-approval')?.value;

  // Stats
  document.getElementById('as-total').textContent = pOrders.length;
  document.getElementById('as-pending').textContent = pOrders.filter(o=>!o.adminStatus).length;
  document.getElementById('as-approved').textContent = pOrders.filter(o=>o.adminStatus==='approved').length;
  document.getElementById('as-denied').textContent = pOrders.filter(o=>o.adminStatus==='denied').length;

  if (activeAdminTab === 'review') renderAdminReview(filterProject, filterApproval);
  if (activeAdminTab === 'history') renderAdminHistory();
}

function renderAdminReview(filterProject, filterApproval) {
  const el = document.getElementById('admin-review-list'); el.innerHTML = '';

  let orders = [...pOrders];
  if (filterProject) orders = orders.filter(o => String(o.projectId) === String(filterProject));
  if (filterApproval === 'pending_review') orders = orders.filter(o => !o.adminStatus);
  else if (filterApproval === 'approved') orders = orders.filter(o => o.adminStatus === 'approved');
  else if (filterApproval === 'denied') orders = orders.filter(o => o.adminStatus === 'denied');

  // Group by project
  const projectIds = [...new Set(orders.map(o => o.projectId))];

  if (!orders.length) {
    el.innerHTML = '<div class="empty"><div class="empty-icon">📋</div>No orders match this filter.</div>';
    return;
  }

  projectIds.forEach(projId => {
    const proj = pProjects.find(p => p.id === projId);
    const projOrders = orders.filter(o => o.projectId === projId);
    const isCollapsed = adminCollapsed[projId];
    const pendingCount = projOrders.filter(o => !o.adminStatus).length;

    const section = document.createElement('div');
    section.className = 'admin-project-section';

    const header = document.createElement('div');
    header.className = 'admin-project-header' + (isCollapsed ? ' collapsed' : '');
    header.onclick = () => toggleAdminProject(projId);
    header.innerHTML = `
      <span style="font-size:20px">🏗️</span>
      <h4>${proj ? proj.name : 'Unknown Project'}</h4>
      ${pendingCount > 0 ? `<span class="proj-order-count">⏳ ${pendingCount} pending</span>` : ''}
      <span class="proj-order-count">${projOrders.length} order${projOrders.length!==1?'s':''}</span>
      <span class="proj-chevron">▼</span>`;
    section.appendChild(header);

    if (!isCollapsed) {
      projOrders.sort((a,b) => {
        const rank = {undefined:0, denied:1, approved:2};
        return (rank[a.adminStatus]??0) - (rank[b.adminStatus]??0);
      }).forEach(o => {
        const vendor = o.vendorId ? pVendors.find(v=>v.id===o.vendorId) : null;
        const statusLabel = o.adminStatus==='approved' ? '✅ Approved' : o.adminStatus==='denied' ? '❌ Denied' : '⏳ Awaiting Review';
        const statusClass = o.adminStatus==='approved' ? 'status-approved' : o.adminStatus==='denied' ? 'status-denied' : 'status-pending';

        const card = document.createElement('div');
        card.className = 'admin-order-card';

        const materialsHtml = o.materials.map(m =>
          `<div class="admin-mat-row"><span>📦 ${m.name}</span><span class="order-qty">${m.qty||'—'}</span></div>`
        ).join('');

        let actionHtml = '';
        if (!o.adminStatus) {
          actionHtml = `
            <div class="admin-note-row">
              <textarea id="deny-reason-${o.id}" class="admin-deny-reason" placeholder="Reason for denial (optional, fill before denying)..." rows="2"></textarea>
            </div>
            <div class="admin-action-row">
              <button class="btn-approve" onclick="adminApprove(${o.id})">✅ Approve</button>
              <button class="btn-deny" onclick="adminDeny(${o.id})">❌ Deny</button>
            </div>`;
        } else {
          actionHtml = `<div class="admin-action-row">
            <span class="status-badge ${statusClass}">${statusLabel}</span>
            ${o.adminDecidedAt ? `<span style="font-size:11px;color:#aaa;">on ${o.adminDecidedAt}</span>` : ''}
            <button onclick="adminReset(${o.id})" style="font-size:11px;padding:4px 10px;border-radius:20px;border:1px solid #ddd;background:#f9f8f6;cursor:pointer;color:#555;font-weight:500;margin-left:auto;">↩️ Reset</button>
          </div>
          ${o.adminReason ? `<div style="font-size:12px;color:#991b1b;background:#fff5f5;border-radius:8px;padding:6px 10px;margin-top:6px;">💬 Reason: ${o.adminReason}</div>` : ''}`;
        }

        card.innerHTML = `
          <div class="admin-order-top">
            <div>
              <div class="admin-order-vendor">${vendor ? '🏪 '+vendor.name+' · 📞 '+vendor.phone : 'No vendor assigned'}</div>
              <div class="admin-order-meta">Added ${o.createdAt}${o.orderedAt?' · Ordered '+o.orderedAt:''}${o.deliveredAt?' · Delivered '+o.deliveredAt:''}</div>
            </div>
            <span class="status-badge ${statusClass}" style="flex-shrink:0">${statusLabel}</span>
          </div>
          <div class="admin-materials">${materialsHtml}</div>
          ${o.notes ? `<div style="font-size:12px;color:#888;padding:6px 10px;background:#f9f8f6;border-radius:8px;">💬 ${o.notes}</div>` : ''}
          ${actionHtml}`;
        section.appendChild(card);
      });
    }
    el.appendChild(section);
  });
}

function renderAdminHistory() {
  const el = document.getElementById('admin-history-list'); el.innerHTML = '';
  const decided = pOrders.filter(o => o.adminStatus).sort((a,b) => (b.adminDecidedAt||'').localeCompare(a.adminDecidedAt||''));
  if (!decided.length) {
    el.innerHTML = '<div class="empty"><div class="empty-icon">📜</div>No decisions made yet.</div>';
    return;
  }
  decided.forEach(o => {
    const proj = pProjects.find(p=>p.id===o.projectId);
    const icon = o.adminStatus==='approved' ? '✅' : '❌';
    const statusClass = o.adminStatus==='approved' ? 'status-approved' : 'status-denied';
    const matSummary = o.materials.map(m=>m.name).join(', ');
    const card = document.createElement('div');
    card.className = 'approval-history-card';
    card.innerHTML = `
      <span class="approval-history-icon">${icon}</span>
      <div class="approval-history-body">
        <div class="approval-history-title">${proj ? proj.name : 'Unknown'} · <span style="font-weight:400;color:#666">${matSummary}</span></div>
        <div class="approval-history-sub">
          <span class="status-badge ${statusClass}">${o.adminStatus==='approved'?'Approved':'Denied'}</span>
          ${o.adminDecidedAt ? ` on ${o.adminDecidedAt}` : ''}
          ${o.adminReason ? ` · Reason: ${o.adminReason}` : ''}
        </div>
      </div>
      <button onclick="adminReset(${o.id})" style="font-size:11px;padding:4px 10px;border-radius:20px;border:1px solid #ddd;background:#f9f8f6;cursor:pointer;color:#555;font-weight:500;flex-shrink:0;">↩️ Reset</button>`;
    el.appendChild(card);
  });
}

function addMaterialRow(name='', qty='') {
  const container = document.getElementById('materials-container');
  const rowId = 'mat-'+Date.now()+'-'+matRowCount++;
  const row = document.createElement('div');
  row.className = 'material-row'; row.id = rowId;
  row.innerHTML = `<input type="text" placeholder="Material name" class="mat-name" value="${name}">
    <input type="text" placeholder="Qty/Unit" class="qty-input mat-qty" value="${qty}">
    <button class="remove-mat-btn" onclick="document.getElementById('${rowId}').remove()">✕</button>`;
  container.appendChild(row);
}

function openOrderModal() {
  if (!pProjects.length) { alert('Please add at least one project first!'); switchPravinTab('projects'); return; }
  populateOrderDropdowns();
  document.getElementById('materials-container').innerHTML = '';
  matRowCount = 0;
  addMaterialRow(); addMaterialRow();
  document.getElementById('order-heading').value = '';
  document.getElementById('order-notes').value = '';
  document.getElementById('order-project').value = '';
  document.getElementById('order-vendor').value = '';
  document.getElementById('order-modal').classList.add('open');
}
function closeOrderModal(e) {
  if (e && e.target !== document.getElementById('order-modal')) return;
  document.getElementById('order-modal').classList.remove('open');
}

function saveOrder() {
  const projectId = parseInt(document.getElementById('order-project').value);
  if (!projectId) { alert('Please select a project!'); return; }
  const vendorId = parseInt(document.getElementById('order-vendor').value) || null;

  const materials = [];
  document.querySelectorAll('.material-row').forEach(row => {
    const n = row.querySelector('.mat-name').value.trim();
    const q = row.querySelector('.mat-qty').value.trim();
    if (n) materials.push({name:n, qty:q});
  });
  if (!materials.length) { alert('Please add at least one material!'); return; }

  const heading = document.getElementById('order-heading').value.trim();
  if (!heading) { alert('Please enter an order heading!'); return; }
  pOrders.push({
    id: Date.now(),
    projectId,
    vendorId,
    heading,
    materials,
    notes: document.getElementById('order-notes').value.trim(),
    status: 'pending',
    createdAt: new Date().toLocaleDateString('en-IN')
  });
  savePravin();
  document.getElementById('order-modal').classList.remove('open');
  showToast('📦 Order added!');
  renderPravin();
}

function markOrdered(id) {
  const o = pOrders.find(o=>o.id===id);
  if (o) { o.status='ordered'; o.orderedAt=new Date().toLocaleDateString('en-IN'); savePravin(); renderPravin(); showToast('✅ Marked as Ordered!'); }
}
function markDelivered(id) {
  const o = pOrders.find(o=>o.id===id);
  if (!o) return;
  o.status="delivered"; o.deliveredAt=new Date().toLocaleDateString("en-IN");
  autoTriggerMilestonesOnDelivery(o);
  savePravin(); renderPravin(); showToast("🎉 Marked as Delivered! Milestones auto-updated.");
}
function deleteOrder(id) {
  if (confirm('Delete this order?')) { pOrders=pOrders.filter(o=>o.id!==id); savePravin(); renderPravin(); }
}

function renderOrders() {
  const el = document.getElementById('orders-list'); el.innerHTML='';
  const filterProject = document.getElementById('order-filter-project')?.value;
  const filterStatus = document.getElementById('order-filter-status')?.value;

  let filtered = [...pOrders];
  if (filterProject) filtered = filtered.filter(o=>String(o.projectId)===String(filterProject));
  if (filterStatus) filtered = filtered.filter(o=>o.status===filterStatus);

  if (!filtered.length) {
    el.innerHTML='<div class="empty"><div class="empty-icon">📦</div>No orders found. Create your first material order!</div>';
    return;
  }

  // Sort: pending first, ordered next, delivered last
  filtered.sort((a,b) => {
    const rank = {pending:0, ordered:1, delivered:2};
    return rank[a.status]-rank[b.status];
  });

  filtered.forEach(o => {
    const proj = pProjects.find(p=>p.id===o.projectId);
    const vendor = o.vendorId ? pVendors.find(v=>v.id===o.vendorId) : null;
    const statusClass = o.status==='pending'?'status-pending':o.status==='ordered'?'status-ordered':'status-delivered';
    const statusLabel = o.status==='pending'?'⏳ Pending':o.status==='ordered'?'📬 Ordered':'✅ Delivered';

    // Admin approval status strip
    let adminStripHtml = '';
    if (!o.adminStatus) {
      adminStripHtml = `<div class="admin-status-strip admin-strip-awaiting">
        <span class="admin-strip-icon">🕐</span>
        <div><div>Awaiting Admin Approval</div><div class="admin-strip-reason">You can proceed once the admin approves this order.</div></div>
      </div>`;
    } else if (o.adminStatus === 'approved') {
      adminStripHtml = `<div class="admin-status-strip admin-strip-approved">
        <span class="admin-strip-icon">✅</span>
        <div><div>Approved by Admin</div><div class="admin-strip-reason">on ${o.adminDecidedAt||''}</div></div>
      </div>`;
    } else if (o.adminStatus === 'denied') {
      adminStripHtml = `<div class="admin-status-strip admin-strip-denied">
        <span class="admin-strip-icon">❌</span>
        <div><div>Denied by Admin</div>${o.adminReason?`<div class="admin-strip-reason">Reason: ${o.adminReason}</div>`:''}</div>
      </div>`;
    }

    // Action buttons — only allow "Mark Ordered" if admin approved
    let actionBtns = '';
    if (o.status === 'pending' && o.adminStatus === 'approved') {
      actionBtns = `<button class="order-action-btn btn-mark-ordered" onclick="markOrdered(${o.id})">📬 Mark Ordered</button>`;
    } else if (o.status === 'pending' && o.adminStatus !== 'approved') {
      // greyed out hint — no button
      actionBtns = `<span style="font-size:11px;color:#bbb;font-style:italic;">Waiting for admin approval to proceed</span>`;
    }
    if (o.status === 'ordered') {
      actionBtns = `<button class="order-action-btn btn-mark-delivered" onclick="markDelivered(${o.id})">✅ Mark Delivered</button>`;
    }

    const materialsHtml = o.materials.map(m=>
      `<div class="order-item-row"><span>${m.name}</span><span class="order-qty">${m.qty||'—'}</span></div>`
    ).join('');

    const d = document.createElement('div');
    d.className = 'order-card';
    // Add a left-border colour based on admin status when order is pending
    if (o.status === 'pending') {
      if (o.adminStatus === 'approved') d.style.borderLeft = '3px solid #22c55e';
      else if (o.adminStatus === 'denied') d.style.borderLeft = '3px solid #ef4444';
      else d.style.borderLeft = '3px solid #f59e0b';
    }
    d.innerHTML = `
      <div class="order-card-header">
        <div>
          ${o.heading ? `<div class="order-card-title">📋 ${o.heading}</div>` : ''}
          <div class="order-card-project">🏗️ ${proj ? proj.name : 'Unknown Project'}</div>
          ${vendor?`<div class="order-card-vendor">🏪 ${vendor.name} · 📞 ${vendor.phone}</div>`:'<div class="order-card-vendor">No vendor assigned</div>'}
          <div style="font-size:11px;color:#aaa;margin-top:2px;">Added ${o.createdAt}${o.orderedAt?' · Ordered '+o.orderedAt:''}${o.deliveredAt?' · Delivered '+o.deliveredAt:''}</div>
        </div>
        <button class="icon-btn del" onclick="deleteOrder(${o.id})" title="Delete">✕</button>
      </div>
      <div class="order-items-list">${materialsHtml}</div>
      ${o.notes?`<div style="font-size:12px;color:#888;margin-top:8px;padding:6px 10px;background:#f9f8f6;border-radius:8px;">💬 ${o.notes}</div>`:''}
      ${adminStripHtml}
      <div class="order-status-row" style="margin-top:10px;">
        <span class="status-badge ${statusClass}">${statusLabel}</span>
        ${actionBtns}
      </div>`;
    el.appendChild(d);
  });
}

// ══════════════════════════════════════════════════
//  MATERIALS REPORT (Project-wise Excel-style table)
// ══════════════════════════════════════════════════
let reportOpenProjects = new Set();

function toggleReportProject(projId) {
  if (reportOpenProjects.has(projId)) reportOpenProjects.delete(projId);
  else reportOpenProjects.add(projId);
  renderMaterialsReport();
}

function renderMaterialsReport() {
  const el = document.getElementById('report-projects-list');
  const summaryEl = document.getElementById('report-summary-bar');
  if (!el) return;
  el.innerHTML = '';

  if (!pProjects.length) {
    el.innerHTML = '<div class="report-empty">📋 No projects yet. Add a project first!</div>';
    summaryEl.innerHTML = '';
    return;
  }

  // Overall summary
  const totalOrders = pOrders.length;
  const totalMats = pOrders.reduce((s,o) => s + (o.materials||[]).length, 0);
  const totalApproved = pOrders.filter(o=>o.adminStatus==='approved').length;
  const totalPending = pOrders.filter(o=>!o.adminStatus).length;
  summaryEl.innerHTML = `
    <div class="report-sum-card"><div class="report-sum-num">${pProjects.length}</div><div class="report-sum-label">Projects</div></div>
    <div class="report-sum-card"><div class="report-sum-num">${totalOrders}</div><div class="report-sum-label">Total Orders</div></div>
    <div class="report-sum-card"><div class="report-sum-num" style="color:#166534">${totalApproved}</div><div class="report-sum-label">Approved</div></div>
    <div class="report-sum-card"><div class="report-sum-num" style="color:#92400e">${totalPending}</div><div class="report-sum-label">Awaiting</div></div>
    <div class="report-sum-card"><div class="report-sum-num" style="color:#1a1a1a">${totalMats}</div><div class="report-sum-label">Materials</div></div>`;

  pProjects.forEach(proj => {
    const projOrders = pOrders.filter(o => o.projectId === proj.id);
    const isOpen = reportOpenProjects.has(proj.id);
    const approvedCount = projOrders.filter(o=>o.adminStatus==='approved').length;
    const pendingCount  = projOrders.filter(o=>!o.adminStatus).length;
    const deniedCount   = projOrders.filter(o=>o.adminStatus==='denied').length;
    const matCount = projOrders.reduce((s,o)=>s+(o.materials||[]).length,0);

    const card = document.createElement('div');
    card.className = 'report-project-card' + (isOpen ? ' open' : '');

    // Header
    const header = document.createElement('div');
    header.className = 'report-project-header';
    header.onclick = () => toggleReportProject(proj.id);
    header.innerHTML = `
      <span class="report-proj-icon">🏗️</span>
      <div class="report-proj-info">
        <div class="report-proj-name">${proj.name}</div>
        <div class="report-proj-meta">${proj.location?'📍 '+proj.location+' · ':''}${projOrders.length} order${projOrders.length!==1?'s':''} · ${matCount} material item${matCount!==1?'s':''}</div>
        <div class="report-proj-badges">
          <span class="report-badge report-badge-total">📦 ${projOrders.length} orders</span>
          ${approvedCount?`<span class="report-badge report-badge-approved">✅ ${approvedCount} approved</span>`:''}
          ${pendingCount?`<span class="report-badge report-badge-pending">⏳ ${pendingCount} pending</span>`:''}
          ${deniedCount?`<span class="report-badge report-badge-denied">❌ ${deniedCount} denied</span>`:''}
        </div>
      </div>
      <span class="report-chevron">▼</span>`;
    card.appendChild(header);

    if (isOpen) {
      if (!projOrders.length) {
        const empty = document.createElement('div');
        empty.className = 'report-empty';
        empty.textContent = 'No orders for this project yet.';
        card.appendChild(empty);
      } else {
        // Build flat rows for table
        const rows = buildProjectRows(proj, projOrders);

        const tableWrap = document.createElement('div');
        tableWrap.className = 'excel-table-wrap';
        tableWrap.innerHTML = buildExcelTableHTML(rows);
        card.appendChild(tableWrap);

        // Export row
        const exportRow = document.createElement('div');
        exportRow.className = 'report-export-row';
        exportRow.innerHTML = `
          <span style="font-size:12px;color:#888;font-weight:500;">Export ${proj.name}:</span>
          <button class="report-export-btn btn-export-excel" onclick="exportProjectExcel(${proj.id})">📗 Download Excel</button>
          <button class="report-export-btn btn-export-csv" onclick="exportProjectCSV(${proj.id})">📄 Download CSV</button>`;
        card.appendChild(exportRow);
      }
    }

    el.appendChild(card);
  });
}

function buildProjectRows(proj, projOrders) {
  const rows = [];
  let sno = 1;
  projOrders.forEach(order => {
    const vendor = order.vendorId ? pVendors.find(v=>v.id===order.vendorId) : null;
    const adminStatus = order.adminStatus || 'pending_review';
    const orderStatus = order.status;
    (order.materials || []).forEach(mat => {
      rows.push({
        sno: sno++,
        project: proj.name,
        location: proj.location || '—',
        material: mat.name || '—',
        qty: mat.qty || '—',
        vendor: vendor ? vendor.name : '—',
        vendorPhone: vendor ? vendor.phone : '—',
        notes: order.notes || '—',
        orderStatus,
        adminStatus,
        adminDecidedAt: order.adminDecidedAt || '—',
        adminReason: order.adminReason || '—',
        createdAt: order.createdAt || '—',
      });
    });
  });
  return rows;
}

function adminStatusLabel(s) {
  if (s === 'approved') return 'Approved';
  if (s === 'denied') return 'Denied';
  return 'Awaiting Review';
}
function orderStatusLabel(s) {
  if (s === 'ordered') return 'Ordered';
  if (s === 'delivered') return 'Delivered';
  return 'Pending';
}
function adminPillClass(s) {
  if (s === 'approved') return 'pill-approved';
  if (s === 'denied') return 'pill-denied';
  return 'pill-pending';
}
function orderPillClass(s) {
  if (s === 'ordered') return 'pill-ordered';
  if (s === 'delivered') return 'pill-delivered';
  return 'pill-pending';
}

function buildExcelTableHTML(rows) {
  const headers = ['#','Material','Qty/Unit','Vendor','Vendor Phone','Order Status','Admin Approval','Decision Date','Notes','Added On'];
  let html = `<table class="excel-table"><thead><tr>${headers.map(h=>`<th>${h}</th>`).join('')}</tr></thead><tbody>`;
  rows.forEach(r => {
    html += `<tr>
      <td class="col-sno">${r.sno}</td>
      <td><b>${r.material}</b></td>
      <td class="col-qty">${r.qty}</td>
      <td>${r.vendor}</td>
      <td>${r.vendorPhone}</td>
      <td class="col-status"><span class="excel-status-pill ${orderPillClass(r.orderStatus)}">${orderStatusLabel(r.orderStatus)}</span></td>
      <td class="col-status"><span class="excel-status-pill ${adminPillClass(r.adminStatus)}">${adminStatusLabel(r.adminStatus)}</span></td>
      <td>${r.adminDecidedAt}</td>
      <td>${r.notes}</td>
      <td>${r.createdAt}</td>
    </tr>`;
  });
  // Totals row
  html += `<tr class="subtotal-row">
    <td colspan="2" style="text-align:right;">TOTAL ITEMS</td>
    <td class="col-qty">${rows.length}</td>
    <td colspan="7"></td>
  </tr>`;
  html += '</tbody></table>';
  return html;
}

// ── EXCEL EXPORT (using SheetJS from CDN) ──
function loadSheetJS(callback) {
  if (window.XLSX) { callback(); return; }
  const script = document.createElement('script');
  script.src = 'https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js';
  script.onload = callback;
  script.onerror = () => { showToast('⚠️ Could not load Excel library. Try CSV instead.'); };
  document.head.appendChild(script);
}

function exportProjectExcel(projId) {
  loadSheetJS(() => {
    const proj = pProjects.find(p=>p.id===projId);
    const projOrders = pOrders.filter(o=>o.projectId===projId);
    if (!proj || !projOrders.length) { showToast('No data to export.'); return; }
    const rows = buildProjectRows(proj, projOrders);
    const wsData = [
      ['#','Material','Qty/Unit','Vendor','Vendor Phone','Order Status','Admin Approval','Decision Date','Notes','Added On'],
      ...rows.map(r=>[r.sno,r.material,r.qty,r.vendor,r.vendorPhone,orderStatusLabel(r.orderStatus),adminStatusLabel(r.adminStatus),r.adminDecidedAt,r.notes,r.createdAt]),
      ['','TOTAL ITEMS',rows.length,'','','','','','','']
    ];
    const wb = XLSX.utils.book_new();
    const ws = XLSX.utils.aoa_to_sheet(wsData);
    // Column widths
    ws['!cols'] = [5,25,12,20,14,14,16,14,22,12].map(w=>({wch:w}));
    XLSX.utils.book_append_sheet(wb, ws, proj.name.slice(0,31));
    XLSX.writeFile(wb, `Materials_${proj.name.replace(/\s+/g,'_')}.xlsx`);
    showToast('📗 Excel downloaded!');
  });
}

function exportProjectCSV(projId) {
  const proj = pProjects.find(p=>p.id===projId);
  const projOrders = pOrders.filter(o=>o.projectId===projId);
  if (!proj || !projOrders.length) { showToast('No data to export.'); return; }
  const rows = buildProjectRows(proj, projOrders);
  const headers = ['#','Material','Qty/Unit','Vendor','Vendor Phone','Order Status','Admin Approval','Decision Date','Notes','Added On'];
  const csvRows = [headers, ...rows.map(r=>[r.sno,r.material,r.qty,r.vendor,r.vendorPhone,orderStatusLabel(r.orderStatus),adminStatusLabel(r.adminStatus),r.adminDecidedAt,r.notes,r.createdAt])];
  const csv = csvRows.map(row=>row.map(v=>'"'+String(v).replace(/"/g,'""')+'"').join(',')).join('\n');
  const blob = new Blob([csv], {type:'text/csv'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `Materials_${proj.name.replace(/\s+/g,'_')}.csv`;
  a.click();
  showToast('📄 CSV downloaded!');
}

function exportAllProjectsExcel() {
  loadSheetJS(() => {
    if (!pProjects.length) { showToast('No projects to export.'); return; }
    const wb = XLSX.utils.book_new();
    let hasData = false;

    // Summary sheet
    const summaryData = [
      ['Project Name','Location','Total Orders','Materials Count','Approved','Pending Review','Denied'],
      ...pProjects.map(proj => {
        const orders = pOrders.filter(o=>o.projectId===proj.id);
        const mats = orders.reduce((s,o)=>s+(o.materials||[]).length,0);
        return [proj.name, proj.location||'—', orders.length, mats,
          orders.filter(o=>o.adminStatus==='approved').length,
          orders.filter(o=>!o.adminStatus).length,
          orders.filter(o=>o.adminStatus==='denied').length];
      })
    ];
    const wsSummary = XLSX.utils.aoa_to_sheet(summaryData);
    wsSummary['!cols'] = [25,16,14,16,12,16,10].map(w=>({wch:w}));
    XLSX.utils.book_append_sheet(wb, wsSummary, 'Summary');

    pProjects.forEach(proj => {
      const projOrders = pOrders.filter(o=>o.projectId===proj.id);
      if (!projOrders.length) return;
      const rows = buildProjectRows(proj, projOrders);
      const wsData = [
        ['#','Material','Qty/Unit','Vendor','Vendor Phone','Order Status','Admin Approval','Decision Date','Notes','Added On'],
        ...rows.map(r=>[r.sno,r.material,r.qty,r.vendor,r.vendorPhone,orderStatusLabel(r.orderStatus),adminStatusLabel(r.adminStatus),r.adminDecidedAt,r.notes,r.createdAt]),
        ['','TOTAL ITEMS',rows.length,'','','','','','','']
      ];
      const ws = XLSX.utils.aoa_to_sheet(wsData);
      ws['!cols'] = [5,25,12,20,14,14,16,14,22,12].map(w=>({wch:w}));
      XLSX.utils.book_append_sheet(wb, ws, proj.name.slice(0,31));
      hasData = true;
    });

    if (!hasData && pProjects.length) {
      showToast('No order data to export yet.');
      return;
    }
    const today = new Date().toLocaleDateString('en-IN').replace(/\//g,'-');
    XLSX.writeFile(wb, `Materials_Report_All_Projects_${today}.xlsx`);
    showToast('📗 All projects exported!');
  });
}

// ══════════════════════════════════════════════════
//  USERNAME EDIT
// ══════════════════════════════════════════════════
const UN_KEYS = { priyanka:'uname_priyanka', pravin:'uname_pravin', admin:'uname_admin', manish:'uname_manish' };
function getUsername(user) { return localStorage.getItem(UN_KEYS[user]) || USER_META[user].name; }
function setUsername(user, name) { localStorage.setItem(UN_KEYS[user], name); }
function refreshUsernameCards() {
  ['priyanka','pravin','admin','manish'].forEach(u => {
    const el = document.getElementById('uname-'+u); if (el) el.textContent = getUsername(u);
  });
}
function openEditUsername(user) {
  const cur = getUsername(user);
  const newName = prompt(`Edit username for ${USER_META[user].name}:`, cur);
  if (newName && newName.trim()) {
    setUsername(user, newName.trim());
    // Update topbar display
    const dn = document.getElementById(user+'-display-name');
    if (dn) dn.textContent = newName.trim();
    showToast('✓ Username updated!');
  }
}

// ══════════════════════════════════════════════════
//  PRAVIN WORK ITEMS
// ══════════════════════════════════════════════════
const WI_MILESTONES = [
  'Site Masking',
  'Material Procured',
  'Preparing carcass',
  'Laminate selection',
  'Laminate Procured',
  'Hardware procured',
  'Outer laminate Pasting',
  'Handle Selection',
  'Handle Procured',
  'Final finishing',
  'Cleaning and Handover'
];
// milestone keys used for auto-trigger matching
const WI_MILESTONE_KEYS = {
  laminate_procured: 4,   // index of "Laminate Procured"
  hardware_procured: 5,   // index of "Hardware procured"
  handle_procured:   8,   // index of "Handle Procured"
};
// milestone index -1 = not started, 0..10 = steps
function wiPctFromMilestone(ms) {
  if (ms < 0) return 0;
  return Math.round(((ms + 1) / WI_MILESTONES.length) * 100);
}
function wiStatusLabel(wi) {
  if (wi.progMode === 'percent') {
    const p = wi.manualPct || 0;
    if (p === 0) return 'Not Started';
    if (p >= 100) return 'Completed';
    return `${p}% Done`;
  }
  if (wi.milestone == null || wi.milestone < 0) return 'Not Started';
  if (wi.milestone >= WI_MILESTONES.length - 1) return 'Cleaning and Handover';
  return WI_MILESTONES[wi.milestone];
}

// ── Auto-milestone trigger when an order is marked delivered ──
// Checks material names in the delivered order and auto-marks relevant
// milestones (per project) on all work items tied to that project.
function autoTriggerMilestonesOnDelivery(order) {
  if (!order || order.status !== 'delivered') return;
  // Ensure pWorkItems is fresh from storage
  pWorkItems = JSON.parse(localStorage.getItem('pravin_workitems') || '[]');
  const heading = (order.heading || '').toLowerCase();
  const mats = (order.materials || []).map(m => (m.name || '').toLowerCase());
  // Check heading first (primary trigger), then fall back to material names
  const hasLaminate = heading.includes('laminate') || mats.some(n => n.includes('laminate'));
  const hasHardware = heading.includes('hardware') || mats.some(n => n.includes('hardware'));
  const hasHandle   = heading.includes('handle')   || mats.some(n => n.includes('handle'));
  if (!hasLaminate && !hasHardware && !hasHandle) return;

  pWorkItems.forEach(wi => {
    if (wi.projectId && wi.projectId !== order.projectId) return;
    if (wi.progMode !== 'milestone') return;
    let ms = wi.milestone != null ? wi.milestone : -1;
    let changed = false;
    if (hasLaminate && ms < WI_MILESTONE_KEYS.laminate_procured) {
      ms = WI_MILESTONE_KEYS.laminate_procured; changed = true;
    }
    if (hasHardware && ms < WI_MILESTONE_KEYS.hardware_procured) {
      ms = WI_MILESTONE_KEYS.hardware_procured; changed = true;
    }
    if (hasHandle && ms < WI_MILESTONE_KEYS.handle_procured) {
      ms = WI_MILESTONE_KEYS.handle_procured; changed = true;
    }
    if (changed) {
      wi.milestone = ms;
      wi.updatedAt = new Date().toLocaleString('en-IN',{day:'2-digit',month:'short',year:'numeric',hour:'2-digit',minute:'2-digit'});
    }
  });
  savePravin();
}
function wiEffectivePct(wi) {
  if (wi.progMode === 'percent') return wi.manualPct || 0;
  return wiPctFromMilestone(wi.milestone != null ? wi.milestone : -1);
}

let wiOpenItems = new Set();
let editingWiId = null;

function renderWorkItems() {
  pWorkItems = JSON.parse(localStorage.getItem('pravin_workitems') || '[]');
  // Populate project filter
  const pf = document.getElementById('wi-filter-project');
  if (pf) {
    const cur = pf.value;
    pf.innerHTML = '<option value="">All Projects</option>' +
      pProjects.map(p=>`<option value="${p.id}"${String(p.id)===cur?' selected':''}>${p.name}</option>`).join('');
  }

  const filterProj = document.getElementById('wi-filter-project')?.value;
  const filterCat  = document.getElementById('wi-filter-cat')?.value;
  const filterType = document.getElementById('wi-filter-type')?.value;

  let list = [...pWorkItems];
  if (filterProj) list = list.filter(w => String(w.projectId) === String(filterProj));
  if (filterCat)  list = list.filter(w => w.category === filterCat);
  if (filterType) list = list.filter(w => w.itemType === filterType);

  // Stats
  const total = pWorkItems.length;
  const done  = pWorkItems.filter(w => wiEffectivePct(w) >= 100).length;
  const inprog= pWorkItems.filter(w => { const p=wiEffectivePct(w); return p>0 && p<100; }).length;
  const nostart = pWorkItems.filter(w => wiEffectivePct(w) === 0).length;
  document.getElementById('wi-s-total').textContent = total;
  document.getElementById('wi-s-done').textContent = done;
  document.getElementById('wi-s-inprog').textContent = inprog;
  document.getElementById('wi-s-notstart').textContent = nostart;

  const el = document.getElementById('work-items-list'); el.innerHTML = '';
  if (!list.length) {
    el.innerHTML = '<div class="empty"><div class="empty-icon">📋</div>No work items yet. Add your first one!</div>'; return;
  }

  list.forEach(wi => {
    const isOpen = wiOpenItems.has(wi.id);
    const proj = pProjects.find(p => p.id === wi.projectId);
    const pct = wiEffectivePct(wi);
    const statusLbl = wiStatusLabel(wi);
    const updatedAt = wi.updatedAt || wi.createdAt || '';

    // Type badge colour
    const typeCls = wi.itemType==='Production'?'wi-badge-type-prod':wi.itemType==='Raw Material'?'wi-badge-type-raw':'wi-badge-type-site';
    const statusCls = wi.status==='Confirmed'?'wi-badge-status-conf':'wi-badge-status-draft';

    // Tasks progress
    const tasks = wi.tasks || [];
    const taskDone = tasks.filter(t=>t.done).length;

    const card = document.createElement('div');
    card.className = 'wi-item-card';

    // Build progress section
    let progressSection = '';
    if (wi.progMode === 'milestone') {
      const ms = wi.milestone != null ? wi.milestone : -1;
      const fillPct = ms < 0 ? 0 : ((ms + 0.5) / WI_MILESTONES.length * 100);
      const dots = WI_MILESTONES.map((lbl, i) => {
        const isDone = i <= ms;
        const isActive = i === ms;
        const dotCls = isDone ? 'done' : (isActive ? 'active' : '');
        const lblCls = isDone ? 'done' : (isActive ? 'active' : '');
        return `<div class="wi-milestone-step" onclick="event.stopPropagation();setWiMilestone(${wi.id},${i})" title="Set: ${lbl}">
          <div class="wi-step-dot ${dotCls}"></div>
          <div class="wi-step-label ${lblCls}">${lbl}</div>
        </div>`;
      }).join('');
      const trackWidth = ms < 0 ? 0 : Math.round((ms / (WI_MILESTONES.length - 1)) * 100);
      progressSection = `
        <div class="wi-progress-label">Progress Status: <b>${statusLbl}</b> <span style="font-size:10px;color:#aaa;font-weight:400;">&nbsp;·&nbsp;${pct}%</span></div>
        <div class="wi-milestone-wrap">
          <div class="wi-milestone-track">
            <div class="wi-milestone-fill" style="width:calc(${trackWidth}% - 0px)"></div>
            ${dots}
          </div>
        </div>`;
    } else {
      // Simple % bar with 0/25/50/75/100 labels
      const labels = [0, 25, 50, 75, 100];
      const labelHtml = labels.map(l =>
        `<span class="wi-pct-bar-label${Math.abs(pct-l)<=12?'active':''}">${l}%</span>`
      ).join('');
      progressSection = `
        <div class="wi-progress-label">Progress Status: <b>${pct}%</b></div>
        <div class="wi-pct-bar-wrap">
          <div class="wi-pct-bar-track"><div class="wi-pct-bar-fill" style="width:${pct}%"></div></div>
          <div class="wi-pct-bar-labels">${labelHtml}</div>
        </div>`;
    }

    card.innerHTML = `
      <div class="wi-item-header" onclick="toggleWiItem(${wi.id})">
        <div class="wi-item-left">
          <div class="wi-item-name">${wi.name}</div>
          ${wi.desc?`<div class="wi-item-desc">${wi.desc}</div>`:''}
          <div class="wi-item-meta">
            <span class="wi-badge wi-badge-cat">${wi.category||'—'}</span>
            <span class="wi-badge ${statusCls}">${wi.status||'Draft'}</span>
            <span class="wi-badge ${typeCls}">${wi.itemType||'Site Work'}</span>
            <span class="wi-badge wi-badge-uom">${wi.uom||'—'}</span>
            ${wi.qty?`<span class="wi-qty-chip">${wi.qty} ${wi.uom||''}</span>`:''}
            ${proj?`<span style="font-size:10px;color:#888;">🏗️ ${proj.name}</span>`:''}
          </div>
        </div>
        <div class="wi-item-actions" onclick="event.stopPropagation()">
          ${updatedAt?`<span style="font-size:10px;color:#bbb;">🕐 ${updatedAt}</span>`:''}
          <button class="icon-btn edit" onclick="openWiModal(${wi.id})" title="Edit">✏️</button>
          <button class="icon-btn del" onclick="deleteWiItem(${wi.id})" title="Delete">✕</button>
        </div>
      </div>
      ${progressSection}
      ${isOpen ? buildWiBody(wi, pct) : ''}`;
    el.appendChild(card);
  });
}

function buildWiBody(wi, pct) {
  const tasks = wi.tasks || [];
  const notes = wi.notes || [];
  const taskDone = tasks.filter(t=>t.done).length;

  const tasksHtml = tasks.length ? tasks.map((t,i) => `
    <div class="wi-task-row">
      <button class="wi-task-check${t.done?' done':''}" onclick="toggleWiTask(${wi.id},${i})">
        ${t.done?'✓':''}
      </button>
      <span class="wi-task-name${t.done?' done':''}">${t.name}</span>
      <span class="wi-task-date">${t.deadline||''}</span>
      <button class="wi-task-del" onclick="deleteWiTask(${wi.id},${i})">✕</button>
    </div>`).join('') : '<div style="font-size:12px;color:#bbb;padding:4px 0">No sub-tasks yet.</div>';

  const notesHtml = notes.map((n,i) => `
    <div class="wi-note-row">
      <span class="wi-note-text">${n.text}</span>
      <span class="wi-note-meta">${n.date}</span>
      <button class="wi-note-del" onclick="deleteWiNote(${wi.id},${i})">✕</button>
    </div>`).join('');

  // Inline milestone/pct control
  let progCtrl = '';
  if (wi.progMode === 'milestone') {
    const ms = wi.milestone != null ? wi.milestone : -1;
    const opts = [['Not Started',-1],...WI_MILESTONES.map((l,i)=>[l,i])].map(([l,v])=>
      `<option value="${v}"${v===ms?' selected':''}>${l}</option>`).join('');
    progCtrl = `<div class="wi-body-section">
      <div class="wi-body-title">🏁 Update Milestone</div>
      <select class="wi-milestone-select" onchange="setWiMilestone(${wi.id},parseInt(this.value))">${opts}</select>
    </div>`;
  } else {
    progCtrl = `<div class="wi-body-section">
      <div class="wi-body-title">📊 Update Completion %</div>
      <div class="wi-pct-manual-row">
        <input class="wi-pct-inp" type="number" min="0" max="100" value="${wi.manualPct||0}" onchange="setWiPct(${wi.id},this.value)">
        <span class="wi-pct-auto-tag">% complete</span>
      </div>
    </div>`;
  }

  return `<div class="wi-item-body">
    <div class="wi-body-section">
      <div class="wi-body-title">📝 Description</div>
      <textarea class="wi-desc-edit" id="wi-desc-${wi.id}" rows="2" placeholder="Add a description for this work item..." onblur="saveWiDesc(${wi.id})">${(wi.desc||'').replace(/`/g,"'")}</textarea>
    </div>
    ${progCtrl}
    <div class="wi-body-section">
      <div class="wi-body-title">✅ Sub-Tasks (${taskDone}/${tasks.length})</div>
      <div class="wi-tasks-list">${tasksHtml}</div>
      <div class="wi-add-task-row">
        <input class="wi-task-inp" id="wi-ti-${wi.id}" placeholder="Add sub-task..." onkeydown="if(event.key==='Enter')addWiTask(${wi.id})">
        <input class="wi-task-date-inp" id="wi-td-${wi.id}" type="date" title="Deadline">
        <button class="wi-task-add-btn" onclick="addWiTask(${wi.id})">+ Add</button>
      </div>
    </div>
    <div class="wi-body-section">
      <div class="wi-body-title">💬 Notes / Updates</div>
      ${notesHtml}
      <div class="wi-add-note-row">
        <input class="wi-note-inp" id="wi-ni-${wi.id}" placeholder="Add a note or update..." onkeydown="if(event.key==='Enter')addWiNote(${wi.id})">
        <button class="wi-note-add-btn" onclick="addWiNote(${wi.id})">+ Note</button>
      </div>
    </div>
  </div>`;
}

function toggleWiItem(id) {
  if (wiOpenItems.has(id)) wiOpenItems.delete(id); else wiOpenItems.add(id);
  renderWorkItems();
}
function setWiMilestone(id, ms) {
  const w = pWorkItems.find(w=>w.id===id); if(!w) return;
  w.milestone = ms;
  w.updatedAt = new Date().toLocaleString('en-IN',{day:'2-digit',month:'short',year:'numeric',hour:'2-digit',minute:'2-digit'});
  savePravin(); renderWorkItems();
}
function setWiPct(id, val) {
  const w = pWorkItems.find(w=>w.id===id); if(!w) return;
  w.manualPct = Math.max(0,Math.min(100,parseInt(val)||0));
  w.updatedAt = new Date().toLocaleString('en-IN',{day:'2-digit',month:'short',year:'numeric',hour:'2-digit',minute:'2-digit'});
  savePravin(); renderWorkItems();
}
function toggleWiTask(id, idx) {
  const w = pWorkItems.find(w=>w.id===id); if(!w) return;
  w.tasks[idx].done = !w.tasks[idx].done;
  savePravin(); renderWorkItems();
}
function deleteWiTask(id, idx) {
  const w = pWorkItems.find(w=>w.id===id); if(!w) return;
  w.tasks.splice(idx,1); savePravin(); renderWorkItems();
}
function addWiTask(id) {
  const w = pWorkItems.find(wi=>wi.id===id); if(!w) return;
  const inp = document.getElementById('wi-ti-'+id);
  const di  = document.getElementById('wi-td-'+id);
  const name = inp ? inp.value.trim() : '';
  if (!name) { showToast('Enter a task name'); return; }
  if (!w.tasks) w.tasks = [];
  w.tasks.push({ name, deadline: di?di.value:'', done:false });
  savePravin(); renderWorkItems();
}
function addWiNote(id) {
  const w = pWorkItems.find(wi=>wi.id===id); if(!w) return;
  const inp = document.getElementById('wi-ni-'+id);
  const text = inp ? inp.value.trim() : '';
  if (!text) return;
  if (!w.notes) w.notes = [];
  w.notes.push({ text, date: new Date().toLocaleDateString('en-IN') });
  savePravin(); renderWorkItems();
}
function deleteWiNote(id, idx) {
  const w = pWorkItems.find(w=>w.id===id); if(!w) return;
  w.notes.splice(idx,1); savePravin(); renderWorkItems();
}
function saveWiDesc(id) {
  const w = pWorkItems.find(w=>w.id===id); if(!w) return;
  const el = document.getElementById("wi-desc-"+id);
  if (!el) return;
  w.desc = el.value.trim();
  savePravin();
  // update the collapsed preview without full re-render
  const card = el.closest(".wi-item-card");
  if (card) { const preview = card.querySelector(".wi-item-desc"); if(preview) preview.textContent = w.desc; }
}
function deleteWiItem(id) {
  if (confirm('Delete this work item?')) {
    pWorkItems = pWorkItems.filter(w=>w.id!==id);
    savePravin(); renderWorkItems();
  }
}

function openWiModal(id) {
  editingWiId = id || null;
  const w = id ? pWorkItems.find(w=>w.id===id) : null;
  document.getElementById('wi-modal-title').textContent = id ? '✏️ Edit Work Item' : '📋 Add Work Item';
  document.getElementById('wi-name').value = w ? w.name : '';
  document.getElementById('wi-desc').value = w ? (w.desc||'') : '';
  document.getElementById('wi-cat').value = w ? (w.category||'Flooring') : 'Flooring';
  document.getElementById('wi-status').value = w ? (w.status||'Draft') : 'Draft';
  document.getElementById('wi-type').value = w ? (w.itemType||'Site Work') : 'Site Work';
  document.getElementById('wi-uom').value = w ? (w.uom||'SQM') : 'SQM';
  document.getElementById('wi-qty').value = w ? (w.qty||'') : '';
  document.getElementById('wi-prog-mode').value = w ? (w.progMode||'milestone') : 'milestone';
  document.getElementById('wi-milestone').value = w ? (w.milestone != null ? w.milestone : -1) : -1;
  document.getElementById('wi-pct').value = w ? (w.manualPct||0) : 0;
  // Project dropdown
  const ps = document.getElementById('wi-project');
  ps.innerHTML = '<option value="">-- No Project --</option>' +
    pProjects.map(p=>`<option value="${p.id}"${w&&w.projectId===p.id?' selected':''}>${p.name}</option>`).join('');
  toggleWiProgMode();
  document.getElementById('wi-modal').classList.add('open');
  setTimeout(()=>document.getElementById('wi-name').focus(),100);
}
function closeWiModal(e) {
  if (e && e.target !== document.getElementById('wi-modal')) return;
  document.getElementById('wi-modal').classList.remove('open');
  editingWiId = null;
}
function toggleWiProgMode() {
  const mode = document.getElementById('wi-prog-mode').value;
  document.getElementById('wi-milestone-group').style.display = mode==='milestone' ? '' : 'none';
  document.getElementById('wi-pct-group').style.display = mode==='percent' ? '' : 'none';
}
function saveWiItem() {
  const name = document.getElementById('wi-name').value.trim();
  if (!name) { showToast('Item name is required!'); return; }
  const mode = document.getElementById('wi-prog-mode').value;
  const projVal = document.getElementById('wi-project').value;
  const now = new Date().toLocaleString('en-IN',{day:'2-digit',month:'short',year:'numeric',hour:'2-digit',minute:'2-digit'});
  const data = {
    name,
    desc: document.getElementById('wi-desc').value.trim(),
    category: document.getElementById('wi-cat').value,
    status: document.getElementById('wi-status').value,
    itemType: document.getElementById('wi-type').value,
    uom: document.getElementById('wi-uom').value,
    qty: document.getElementById('wi-qty').value,
    projectId: projVal ? parseInt(projVal) : null,
    progMode: mode,
    milestone: mode==='milestone' ? parseInt(document.getElementById('wi-milestone').value) : -1,
    manualPct: mode==='percent' ? Math.max(0,Math.min(100,parseInt(document.getElementById('wi-pct').value)||0)) : null,
    updatedAt: now,
  };
  if (editingWiId) {
    const w = pWorkItems.find(w=>w.id===editingWiId);
    if (w) Object.assign(w, data);
  } else {
    pWorkItems.push({ id:Date.now(), tasks:[], notes:[], createdAt:now, ...data });
  }
  savePravin();
  document.getElementById('wi-modal').classList.remove('open');
  editingWiId = null;
  showToast(editingWiId ? '✓ Work item updated!' : '✓ Work item added!');
  renderWorkItems();
}

// ══════════════════════════════════════════════════
//  PRAVIN PROJECT STATUS TAB
// ══════════════════════════════════════════════════
let psOpenProjects = new Set();

function saveProjectStatusData() {
  localStorage.setItem('pravin_projects', JSON.stringify(pProjects));
}

function togglePsProject(projId) {
  if (psOpenProjects.has(projId)) psOpenProjects.delete(projId);
  else psOpenProjects.add(projId);
  renderProjectStatus();
}

function renderProjectStatus() {
  const el = document.getElementById('project-status-list'); if (!el) return;
  el.innerHTML = '';
  if (!pProjects.length) {
    el.innerHTML = '<div class="empty"><div class="empty-icon">🏗️</div>No projects yet. Add projects first!</div>'; return;
  }
  pProjects.forEach(proj => {
    if (!proj.tasks) proj.tasks = [];
    if (!proj.discussions) proj.discussions = [];
    const isOpen = psOpenProjects.has(proj.id);
    const tasks = proj.tasks;
    const doneCount = tasks.filter(t=>t.done).length;
    // Use manual pct if set, else auto
    let pct = (proj.manualPct !== undefined && proj.manualPct !== null) ? proj.manualPct : (tasks.length ? Math.round(doneCount/tasks.length*100) : 0);

    const card = document.createElement('div');
    card.className = 'ps-proj-card' + (isOpen ? ' ps-open' : '');

    const orderCount = pOrders.filter(o=>o.projectId===proj.id).length;

    card.innerHTML = `
      <div class="ps-proj-header${isOpen?' open-bg':''}" onclick="togglePsProject(${proj.id})">
        <span style="font-size:22px">🏗️</span>
        <div class="ps-proj-left">
          <div class="ps-proj-name">${proj.name}</div>
          <div class="ps-proj-meta">${proj.location?'📍 '+proj.location+' · ':''}${tasks.length} tasks · ${orderCount} orders</div>
          <div class="ps-progress-row">
            <div class="ps-bar-bg"><div class="ps-bar-fill" style="width:${pct}%"></div></div>
            <span class="ps-pct">${pct}%</span>
          </div>
        </div>
        <span class="ps-chevron">▼</span>
      </div>
      ${isOpen ? buildPsProjectBody(proj, pct) : ''}`;
    el.appendChild(card);
  });
}

function buildPsProjectBody(proj, pct) {
  const tasks = proj.tasks || [];
  const discussions = proj.discussions || [];
  const tasksHtml = tasks.length ? tasks.map((t,i) => `
    <div class="ps-task-row">
      <button class="ps-check${t.done?' done':''}" onclick="togglePsTask(${proj.id},${i})">
        ${t.done ? '✓' : ''}
      </button>
      <span class="ps-task-name${t.done?' done':''}">${t.name}</span>
      <span class="ps-task-date">${t.deadline||''}</span>
      <button class="ps-disc-del" onclick="deletePsTask(${proj.id},${i})">✕</button>
    </div>`).join('') : '<div style="font-size:12px;color:#bbb;padding:8px 0;">No tasks yet.</div>';

  const discHtml = discussions.map((d,i) => `
    <div class="ps-disc-note">
      <span class="m-note-text">${d.text}</span>
      <span class="ps-disc-meta">${d.date}</span>
      <button class="ps-disc-del" onclick="deletePsNote(${proj.id},${i})">✕</button>
    </div>`).join('');

  return `<div class="ps-proj-body">
    <div class="ps-pct-row" style="margin-top:10px;">
      <span style="font-size:11px;font-weight:700;color:#888;text-transform:uppercase;letter-spacing:0.4px;">Completion %</span>
      <input class="ps-pct-input" type="number" min="0" max="100" value="${pct}" onchange="setPsManualPct(${proj.id},this.value)">
      <span style="font-size:11px;color:#aaa;">(manual override)</span>
    </div>
    <div class="ps-tasks-title" style="font-size:11px;font-weight:700;color:#888;text-transform:uppercase;letter-spacing:0.4px;margin:10px 0 6px;">Tasks</div>
    ${tasksHtml}
    <div class="ps-add-task-row">
      <input class="ps-task-input" id="ps-ti-${proj.id}" placeholder="Add task..." onkeydown="if(event.key==='Enter')addPsTask(${proj.id})">
      <input class="ps-task-date-input" id="ps-td-${proj.id}" type="date" title="Task deadline">
      <button class="ps-task-add-btn" onclick="addPsTask(${proj.id})">+ Add</button>
    </div>
    <div class="ps-disc-section">
      <div class="ps-disc-title">💬 Discussions / Status Notes</div>
      ${discHtml}
      <div class="ps-add-note-row">
        <input class="ps-note-input" id="ps-ni-${proj.id}" placeholder="Add a note or discussion..." onkeydown="if(event.key==='Enter')addPsNote(${proj.id})">
        <button class="ps-note-add-btn" onclick="addPsNote(${proj.id})">+ Note</button>
      </div>
    </div>
  </div>`;
}

function togglePsTask(projId, idx) {
  const p = pProjects.find(p=>p.id===projId); if (!p) return;
  p.tasks[idx].done = !p.tasks[idx].done;
  if (p.manualPct === undefined || p.manualPct === null) { /* auto recalc */ }
  saveProjectStatusData(); renderProjectStatus();
}
function deletePsTask(projId, idx) {
  const p = pProjects.find(p=>p.id===projId); if (!p) return;
  p.tasks.splice(idx,1); saveProjectStatusData(); renderProjectStatus();
}
function addPsTask(projId) {
  const p = pProjects.find(pr=>pr.id===projId); if (!p) return;
  const inp = document.getElementById('ps-ti-'+projId);
  const dateInp = document.getElementById('ps-td-'+projId);
  const name = inp ? inp.value.trim() : '';
  if (!name) { showToast('Enter a task name'); return; }
  if (!p.tasks) p.tasks = [];
  p.tasks.push({ name, deadline: dateInp ? dateInp.value : '', done: false });
  saveProjectStatusData(); renderProjectStatus();
}
function addPsNote(projId) {
  const p = pProjects.find(pr=>pr.id===projId); if (!p) return;
  const inp = document.getElementById('ps-ni-'+projId);
  const text = inp ? inp.value.trim() : '';
  if (!text) return;
  if (!p.discussions) p.discussions = [];
  p.discussions.push({ text, date: new Date().toLocaleDateString('en-IN') });
  saveProjectStatusData(); renderProjectStatus();
}
function deletePsNote(projId, idx) {
  const p = pProjects.find(p=>p.id===projId); if (!p) return;
  p.discussions.splice(idx,1); saveProjectStatusData(); renderProjectStatus();
}
function setPsManualPct(projId, val) {
  const p = pProjects.find(p=>p.id===projId); if (!p) return;
  p.manualPct = Math.max(0, Math.min(100, parseInt(val)||0));
  saveProjectStatusData(); renderProjectStatus();
}

// ══════════════════════════════════════════════════
//  MANISH DASHBOARD
// ══════════════════════════════════════════════════
let mProjects = [];
let editingMProjId = null;
let activeManishTab = 'all';
let mOpenProjects = new Set();

function saveManish() { localStorage.setItem('manish_projects', JSON.stringify(mProjects)); }

function initManish() {
  mProjects = JSON.parse(localStorage.getItem('manish_projects') || '[]');
  document.getElementById('manish-date').textContent = new Date().toLocaleDateString('en-IN',{weekday:'long',year:'numeric',month:'long',day:'numeric'});
  const dn = document.getElementById('manish-display-name'); if (dn) dn.textContent = getUsername('manish');
  renderManish();
}

function switchManishTab(tab) {
  activeManishTab = tab;
  document.querySelectorAll('.m-tab').forEach((b,i) => {
    b.classList.toggle('active', ['all','pending','upcoming','current','completed'][i] === tab);
  });
  const titles = {all:'🗂 All Projects',pending:'⏳ Pending Projects',upcoming:'📅 Upcoming Projects',current:'🔨 Current Projects',completed:'✅ Completed Projects'};
  document.getElementById('manish-tab-title').textContent = titles[tab]||'Projects';
  renderManish();
}

function renderManish() {
  mProjects = JSON.parse(localStorage.getItem('manish_projects') || '[]');
  const today = new Date(); today.setHours(0,0,0,0);
  const overdue = mProjects.filter(p => p.deadline && p.status !== 'completed' && new Date(p.deadline) < today).length;
  document.getElementById('ms-total').textContent = mProjects.length;
  document.getElementById('ms-current').textContent = mProjects.filter(p=>p.status==='current').length;
  document.getElementById('ms-pending').textContent = mProjects.filter(p=>p.status==='pending').length;
  document.getElementById('ms-overdue').textContent = overdue;
  renderManishProjects();
}

function renderManishProjects() {
  const el = document.getElementById('manish-projects-list'); el.innerHTML = '';
  let list = [...mProjects];
  if (activeManishTab !== 'all') list = list.filter(p => p.status === activeManishTab);
  if (!list.length) {
    el.innerHTML = `<div class="empty"><div class="empty-icon">✏️</div>No projects here yet. Add one!</div>`; return;
  }
  const today = new Date(); today.setHours(0,0,0,0);
  list.forEach(proj => {
    if (!proj.tasks) proj.tasks = [];
    if (!proj.discussions) proj.discussions = [];
    const isOpen = mOpenProjects.has(proj.id);
    const tasks = proj.tasks;
    const doneCount = tasks.filter(t=>t.done).length;
    let pct = (proj.manualPct !== undefined && proj.manualPct !== null) ? proj.manualPct : (tasks.length ? Math.round(doneCount/tasks.length*100) : 0);

    const statusLabels = {pending:'⏳ Pending',upcoming:'📅 Upcoming',current:'🔨 Current',completed:'✅ Completed',onhold:'⏸️ On Hold'};
    const statusClasses = {pending:'m-pill-pending',upcoming:'m-pill-upcoming',current:'m-pill-current',completed:'m-pill-completed',onhold:'m-pill-onhold'};

    let deadlineBadge = '';
    if (proj.deadline) {
      const dl = new Date(proj.deadline); dl.setHours(0,0,0,0);
      const diff = Math.round((dl-today)/(1000*60*60*24));
      if (proj.status === 'completed') deadlineBadge = `<span class="m-deadline-badge m-deadline-ok">✅ Deadline: ${proj.deadline}</span>`;
      else if (diff < 0) deadlineBadge = `<span class="m-deadline-badge m-deadline-overdue">🔴 Overdue by ${-diff}d</span>`;
      else if (diff <= 3) deadlineBadge = `<span class="m-deadline-badge m-deadline-warn">⚠️ Due in ${diff}d</span>`;
      else deadlineBadge = `<span class="m-deadline-badge m-deadline-ok">📅 Due in ${diff}d</span>`;
    } else {
      deadlineBadge = `<span class="m-deadline-badge m-deadline-none">No deadline</span>`;
    }

    const card = document.createElement('div');
    card.className = 'm-project-card';

    const tasksHtml = isOpen ? (tasks.length ? tasks.map((t,i) => `
      <div class="m-task-row">
        <button class="m-task-check${t.done?' done':''}" onclick="toggleMTask(${proj.id},${i})">
          ${t.done ? '✓' : ''}
        </button>
        <span class="m-task-name${t.done?' done':''}">${t.name}</span>
        <span class="m-task-deadline">${t.deadline||''}</span>
        <button class="m-task-del" onclick="deleteMTask(${proj.id},${i})">✕</button>
      </div>`).join('') : '<div style="font-size:12px;color:#bbb;padding:6px 0">No tasks yet.</div>') : '';

    const discHtml = isOpen ? (proj.discussions.map((d,i) => `
      <div class="m-discussion-note">
        <span class="m-note-text">${d.text}</span>
        <span class="m-note-meta">${d.date}</span>
        <button class="m-note-del" onclick="deleteMNote(${proj.id},${i})">✕</button>
      </div>`).join('')) : '';

    card.innerHTML = `
      <div class="m-proj-header">
        <span class="m-proj-icon">✏️</span>
        <div class="m-proj-info">
          <div class="m-proj-name">${proj.name}</div>
          <div class="m-proj-meta">
            <span class="m-status-pill ${statusClasses[proj.status]||'m-pill-pending'}">${statusLabels[proj.status]||proj.status}</span>
            ${proj.client ? `<span>👤 ${proj.client}</span>` : ''}
            ${proj.start ? `<span>🗓 Start: ${proj.start}</span>` : ''}
          </div>
        </div>
        <div class="m-proj-actions">
          <button class="icon-btn edit" onclick="openMProjectModal(${proj.id})" title="Edit">✏️</button>
          <button class="icon-btn del" onclick="deleteMProject(${proj.id})" title="Delete">✕</button>
        </div>
      </div>
      <div class="m-deadline-row">${deadlineBadge}</div>
      <div class="m-progress-row">
        <div class="m-progress-bar-bg"><div class="m-progress-bar-fill" style="width:${pct}%"></div></div>
        <span class="m-progress-pct">${pct}%</span>
      </div>
      <button onclick="toggleMProject(${proj.id})" style="font-size:12px;border:none;background:none;cursor:pointer;color:#7c3aed;font-weight:600;padding:4px 0;">${isOpen?'▲ Collapse':'▼ Tasks & Notes'}</button>
      ${isOpen ? `
      <div class="m-tasks-section">
        <div class="m-tasks-title"><span>📋 Tasks (${doneCount}/${tasks.length})</span></div>
        ${tasksHtml}
        <div class="m-add-task-row">
          <input class="m-task-input" id="mti-${proj.id}" placeholder="Add task..." onkeydown="if(event.key==='Enter')addMTask(${proj.id})">
          <input class="m-task-date-input" id="mtd-${proj.id}" type="date" title="Task deadline">
          <button class="m-task-add-btn" onclick="addMTask(${proj.id})">+ Add</button>
        </div>
      </div>
      <div class="m-pct-row" style="margin-top:10px;">
        <span style="font-size:11px;font-weight:700;color:#888;text-transform:uppercase;letter-spacing:0.4px;">Manual %</span>
        <input class="m-pct-input" type="number" min="0" max="100" value="${pct}" onchange="setMManualPct(${proj.id},this.value)">
      </div>
      <div class="m-discussion-section">
        <div class="m-discussion-title">💬 Discussions / Notes</div>
        ${discHtml}
        <div class="m-add-note-row">
          <input class="m-note-input" id="mni-${proj.id}" placeholder="Add a discussion note..." onkeydown="if(event.key==='Enter')addMNote(${proj.id})">
          <button class="m-note-add-btn" onclick="addMNote(${proj.id})">+ Note</button>
        </div>
      </div>` : ''}`;
    el.appendChild(card);
  });
}

function toggleMProject(projId) {
  if (mOpenProjects.has(projId)) mOpenProjects.delete(projId);
  else mOpenProjects.add(projId);
  renderManish();
}
function toggleMTask(projId, idx) {
  const p = mProjects.find(p=>p.id===projId); if (!p) return;
  p.tasks[idx].done = !p.tasks[idx].done;
  saveManish(); renderManish();
}
function deleteMTask(projId, idx) {
  const p = mProjects.find(p=>p.id===projId); if (!p) return;
  p.tasks.splice(idx,1); saveManish(); renderManish();
}
function addMTask(projId) {
  const p = mProjects.find(pr=>pr.id===projId); if (!p) return;
  const inp = document.getElementById('mti-'+projId);
  const dateInp = document.getElementById('mtd-'+projId);
  const name = inp ? inp.value.trim() : '';
  if (!name) { showToast('Enter a task name'); return; }
  if (!p.tasks) p.tasks = [];
  p.tasks.push({ name, deadline: dateInp ? dateInp.value : '', done: false });
  saveManish(); renderManish();
}
function addMNote(projId) {
  const p = mProjects.find(pr=>pr.id===projId); if (!p) return;
  const inp = document.getElementById('mni-'+projId);
  const text = inp ? inp.value.trim() : '';
  if (!text) return;
  if (!p.discussions) p.discussions = [];
  p.discussions.push({ text, date: new Date().toLocaleDateString('en-IN') });
  saveManish(); renderManish();
}
function deleteMNote(projId, idx) {
  const p = mProjects.find(p=>p.id===projId); if (!p) return;
  p.discussions.splice(idx,1); saveManish(); renderManish();
}
function setMManualPct(projId, val) {
  const p = mProjects.find(p=>p.id===projId); if (!p) return;
  p.manualPct = Math.max(0, Math.min(100, parseInt(val)||0));
  saveManish(); renderManish();
}
function openMProjectModal(id) {
  editingMProjId = id || null;
  const proj = id ? mProjects.find(p=>p.id===id) : null;
  document.getElementById('m-modal-title').textContent = id ? '✏️ Edit Design Project' : '➕ Add Design Project';
  document.getElementById('mp-name').value = proj ? proj.name : '';
  document.getElementById('mp-client').value = proj ? (proj.client||'') : '';
  document.getElementById('mp-status').value = proj ? proj.status : 'pending';
  document.getElementById('mp-start').value = proj ? (proj.start||'') : '';
  document.getElementById('mp-deadline').value = proj ? (proj.deadline||'') : '';
  document.getElementById('mp-notes').value = proj ? (proj.notes||'') : '';
  document.getElementById('m-proj-modal').classList.add('open');
}
function closeMProjectModal() { document.getElementById('m-proj-modal').classList.remove('open'); editingMProjId = null; }
function saveMProject() {
  const name = document.getElementById('mp-name').value.trim();
  if (!name) { showToast('Project name is required!'); return; }
  const data = { name, client: document.getElementById('mp-client').value.trim(), status: document.getElementById('mp-status').value,
    start: document.getElementById('mp-start').value, deadline: document.getElementById('mp-deadline').value, notes: document.getElementById('mp-notes').value.trim() };
  if (editingMProjId) {
    const p = mProjects.find(p=>p.id===editingMProjId);
    if (p) Object.assign(p, data);
  } else {
    mProjects.push({ id: Date.now(), tasks: [], discussions: [], ...data });
  }
  saveManish(); closeMProjectModal(); showToast(editingMProjId ? '✓ Project updated!' : '✓ Project added!'); renderManish();
}
function deleteMProject(id) {
  if (confirm('Delete this design project?')) { mProjects = mProjects.filter(p=>p.id!==id); saveManish(); renderManish(); }
}

</script>
</body>
</html>
