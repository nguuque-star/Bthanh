<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Blox Shop - Random Acc & Admin System</title>
<style>
    :root {
        --bg-body: #050505; --bg-app: #0d1117; --bg-card: #161b22; --bg-input: #21262d;
        --primary: #58a6ff; --primary-text: #ffffff; --secondary-text: #8b949e;
        --danger: #ff4757; --admin-color: #f1c40f; --random-color: #e056fd;
        --success: #2ea043;
        --banner-gradient: linear-gradient(135deg, #8a2be2, #4169e1);
    }
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
    body { background-color: var(--bg-body); color: var(--primary-text); display: flex; justify-content: center; }
    .app-container { width: 100%; max-width: 450px; background-color: var(--bg-app); min-height: 100vh; position: relative; padding-bottom: 70px; overflow-x: hidden; box-shadow: 0 0 20px rgba(0,0,0,0.5); }
    header { display: flex; justify-content: space-between; align-items: center; padding: 15px 20px; background-color: rgba(13, 17, 23, 0.9); position: sticky; top: 0; z-index: 10; border-bottom: 1px solid #30363d; backdrop-filter: blur(10px); }
    .logo { font-size: 18px; font-weight: bold; color: var(--primary); display: flex; align-items: center; gap: 8px; }
    .logo span { animation: sparkle 1.5s ease-in-out infinite; font-size: 20px; }
    .btn-login-top { background: transparent; border: 1px solid var(--primary); color: var(--primary); padding: 6px 12px; border-radius: 20px; font-size: 13px; font-weight: 600; cursor: pointer; display: flex; align-items: center; gap: 6px; transition: 0.2s; }
    .btn-login-top:hover { background: var(--primary); color: #fff; }
    .btn-login-top.is-admin { border-color: var(--admin-color); color: var(--admin-color); }
    .btn-login-top.is-admin:hover { background: var(--admin-color); color: #000; }
    .notice-box { background: rgba(88, 166, 255, 0.1); border: 1px dashed var(--primary); color: #fff; margin: 15px 20px 0 20px; padding: 10px 12px; border-radius: 8px; font-size: 13px; display: flex; align-items: center; gap: 8px; position: relative; overflow: hidden; }
    .notice-box::before {
        content: '';
        position: absolute;
        top: 0; left: -100%;
        width: 100%; height: 100%;
        background: linear-gradient(90deg, transparent, rgba(88,166,255,0.1), transparent);
        animation: shimmer 3s infinite;
    }
    @keyframes shimmer { 0% { left: -100%; } 100% { left: 100%; } }
    .banner { margin: 15px 20px; padding: 20px; border-radius: 12px; background: var(--banner-gradient); text-align: center; box-shadow: 0 4px 15px rgba(88, 166, 255, 0.2); position: relative; overflow: hidden; }
    .banner::after {
        content: ''; position: absolute;
        top: -30px; right: -30px;
        width: 80px; height: 80px;
        background: rgba(255,255,255,0.08);
        border-radius: 50%;
    }
    .banner::before {
        content: ''; position: absolute;
        bottom: -20px; left: -20px;
        width: 60px; height: 60px;
        background: rgba(255,255,255,0.06);
        border-radius: 50%;
    }
    .banner h2 { font-size: 20px; margin-bottom: 5px; text-transform: uppercase; text-shadow: 0 2px 4px rgba(0,0,0,0.3); position: relative; z-index: 1; }
    .banner p { font-size: 13px; color: rgba(255,255,255,0.9); position: relative; z-index: 1; }
    
    /* Mascot GIF */
    .mascot-container { margin: 0 20px 15px 20px; text-align: center; }
    .mascot-gif { max-width: 200px; max-height: 160px; border-radius: 16px; /* box-shadow: 0 4px 20px rgba(88,166,255,0.15); */ display: inline-block; image-rendering: auto; }
    .section { padding: 0 20px; margin-top: 20px; }
    .section-title { font-size: 17px; margin-bottom: 15px; display: flex; align-items: center; gap: 8px; color: #fff; }
    .badge-hot { background: rgba(255, 71, 87, 0.2); color: var(--danger); font-size: 10px; padding: 2px 8px; border-radius: 10px; border: 1px solid var(--danger); animation: glowPulse 2s ease-in-out infinite; }
    .badge-random { background: rgba(224, 86, 253, 0.2); color: var(--random-color); font-size: 10px; padding: 2px 8px; border-radius: 10px; border: 1px solid var(--random-color); animation: glowPulse 2s ease-in-out infinite; }
    .product-grid { display: flex; flex-direction: column; gap: 15px; }
    .card { background-color: var(--bg-card); border: 1px solid #30363d; border-radius: 12px; padding: 15px; position: relative; transition: 0.2s; }
    .card:hover { border-color: #58a6ff55; }
    .card-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 10px; }
    .card-title { font-size: 15px; font-weight: bold; color: #fff; }
    .sold-count { background: rgba(139, 148, 158, 0.1); color: var(--secondary-text); font-size: 11px; padding: 4px 8px; border-radius: 20px; white-space: nowrap; }
    .price-row { display: flex; align-items: baseline; gap: 10px; margin-bottom: 8px; }
    .price-current { font-size: 20px; font-weight: bold; color: var(--primary); }
    .price-old { font-size: 13px; color: var(--secondary-text); text-decoration: line-through; }
    .discount-badge { display: inline-block; background: rgba(255, 71, 87, 0.15); color: var(--danger); padding: 3px 8px; border-radius: 6px; font-size: 11px; font-weight: bold; margin-bottom: 12px; }
    .btn-buy { width: 100%; background: var(--primary); color: #fff; border: none; padding: 12px; border-radius: 8px; font-size: 15px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .btn-random { width: 100%; background: linear-gradient(135deg, #be2edd, #e056fd); color: #fff; border: none; padding: 12px; border-radius: 8px; font-size: 15px; font-weight: bold; cursor: pointer; transition: 0.2s; box-shadow: 0 4px 12px rgba(224, 86, 253, 0.3); }
    .btn-buy:hover, .btn-random:hover { opacity: 0.9; transform: translateY(-1px); }
    .btn-buy:active, .btn-random:active { transform: scale(0.98); }
    /* THANH TOAN */
    .payment-section { margin: 15px 20px 0 20px; padding: 15px; background: rgba(241, 196, 15, 0.08); border: 1px solid var(--admin-color); border-radius: 12px; }
    .payment-title { font-size: 14px; font-weight: bold; color: var(--admin-color); margin-bottom: 10px; display: flex; align-items: center; gap: 6px; }
    .payment-info { display: flex; flex-direction: column; gap: 6px; font-size: 13px; color: #ddd; }
    .payment-row { display: flex; justify-content: space-between; align-items: center; background: var(--bg-input); padding: 8px 12px; border-radius: 6px; gap: 8px; }
    .payment-label { color: var(--secondary-text); white-space: nowrap; }
    .payment-value { font-family: monospace; font-weight: bold; color: #fff; flex: 1; text-align: right; }
    .btn-copy-bank { background: rgba(241, 196, 15, 0.2); border: 1px solid var(--admin-color); color: var(--admin-color); padding: 4px 10px; border-radius: 6px; font-size: 11px; cursor: pointer; transition: 0.2s; white-space: nowrap; }
    .btn-copy-bank:hover { background: var(--admin-color); color: #000; }
    .payment-note { font-size: 11px; color: var(--secondary-text); font-style: italic; margin-top: 6px; }
    /* BOTTOM NAV */
    .bottom-nav { position: fixed; bottom: 0; width: 100%; max-width: 450px; background: var(--bg-card); border-top: 1px solid #30363d; display: flex; justify-content: space-around; padding: 10px 0 15px 0; z-index: 10; }
    .nav-item { display: flex; flex-direction: column; align-items: center; gap: 5px; color: var(--secondary-text); font-size: 11px; cursor: pointer; text-decoration: none; transition: 0.2s; }
    .nav-item.active { color: var(--primary); }
    .nav-item:hover { color: #ddd; }
    .nav-icon { font-size: 20px; display: flex; align-items: center; justify-content: center; }
    /* MODAL */
    .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.7); backdrop-filter: blur(3px); display: none; justify-content: center; align-items: center; z-index: 999; }
    .modal-overlay.active { display: flex; }
    .modal-box { background: var(--bg-card); width: 92%; max-width: 380px; border-radius: 16px; padding: 25px 20px; position: relative; border: 1px solid #30363d; box-shadow: 0 10px 30px rgba(0,0,0,0.5); max-height: 90vh; overflow-y: auto; }
    .btn-close { position: absolute; top: 15px; right: 15px; background: none; border: none; color: var(--secondary-text); font-size: 20px; cursor: pointer; transition: 0.2s; }
    .btn-close:hover { color: #fff; }
    .modal-title { text-align: center; font-size: 22px; color: var(--primary); margin-bottom: 20px; }
    .form-group { margin-bottom: 12px; }
    .form-group label { display: block; font-size: 12px; color: #fff; margin-bottom: 5px; }
    .form-group input, .form-group textarea { width: 100%; padding: 10px 12px; border-radius: 8px; background: var(--bg-input); border: 1px solid #30363d; color: #fff; font-size: 13px; outline: none; }
    .form-group input:focus, .form-group textarea:focus { border-color: var(--primary); }
    .btn-submit { width: 100%; background: var(--primary); color: #fff; border: none; padding: 12px; border-radius: 8px; font-size: 15px; font-weight: bold; margin-top: 10px; cursor: pointer; transition: 0.2s; }
    .btn-submit:hover { opacity: 0.9; }
    .btn-submit:active { transform: scale(0.98); }
    .btn-submit.rainbow-btn { background: linear-gradient(270deg, #ff0000, #ff8800, #ffff00, #00ff00, #0088ff, #4400ff, #ff00ff, #ff0000) !important; background-size: 600% 600% !important; animation: rainbowBg 3s linear infinite !important; color: #fff !important; }
    .btn-submit.rainbow-btn:hover { animation: rainbowBg 1.5s linear infinite !important; transform: translateY(-2px) !important; }
    .btn-card-submit.rainbow-btn { background: linear-gradient(270deg, #ff0000, #ff8800, #ffff00, #00ff00, #0088ff, #4400ff, #ff00ff, #ff0000) !important; background-size: 600% 600% !important; animation: rainbowBg 3s linear infinite !important; color: #fff !important; }
    .btn-card-submit.rainbow-btn:hover { animation: rainbowBg 1.5s linear infinite !important; transform: translateY(-2px) !important; }
    .register-text { text-align: center; font-size: 13px; margin-top: 20px; color: var(--secondary-text); }
    .register-text a { color: var(--primary); text-decoration: none; font-weight: bold; cursor: pointer; }
    .register-text a:hover { text-decoration: underline; }
    .avatar-img-nav { width: 22px; height: 22px; border-radius: 50%; object-fit: cover; border: 1px solid var(--primary); }
    .profile-avatar-container { display: flex; flex-direction: column; align-items: center; gap: 10px; margin-bottom: 20px; }
    .profile-avatar-preview { width: 85px; height: 85px; border-radius: 50%; object-fit: cover; border: 2px solid var(--primary); }
    .profile-username { font-size: 18px; font-weight: bold; color: #fff; margin-bottom: 5px; }
    .admin-badge-title { color: var(--admin-color); font-size: 12px; font-weight: bold; border: 1px solid var(--admin-color); padding: 2px 8px; border-radius: 10px; }
    .btn-secondary { width: 100%; background: transparent; border: 1px solid var(--secondary-text); color: var(--secondary-text); padding: 10px; border-radius: 8px; font-size: 13px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .btn-secondary:hover { border-color: #fff; color: #fff; }
    .btn-danger { width: 100%; background: rgba(255, 71, 87, 0.15); border: 1px solid var(--danger); color: var(--danger); padding: 10px; border-radius: 8px; font-size: 13px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .btn-danger:hover { background: rgba(255, 71, 87, 0.3); }
    .btn-outline-primary { width: 100%; background: transparent; border: 1px solid var(--primary); color: var(--primary); padding: 10px; border-radius: 8px; font-size: 13px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .btn-outline-primary:hover { background: var(--primary); color: #fff; }
    .admin-section { background: var(--bg-input); padding: 12px; border-radius: 10px; margin-bottom: 15px; border: 1px solid #30363d; }
    .admin-section-title { font-size: 13px; font-weight: bold; color: var(--admin-color); margin-bottom: 10px; display: flex; align-items: center; gap: 5px; }
    .admin-prod-card { background: var(--bg-card); border: 1px solid #30363d; border-radius: 8px; padding: 10px; margin-bottom: 10px; }
    .admin-grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
    /* RESULT */
    .result-box { background: var(--bg-input); border: 1px dashed var(--random-color); border-radius: 10px; padding: 15px; text-align: center; margin-bottom: 15px; }
    .result-rank { font-size: 18px; font-weight: bold; color: var(--admin-color); margin-bottom: 10px; }
    .result-field { display: flex; justify-content: space-between; align-items: center; background: var(--bg-card); padding: 10px; border-radius: 6px; margin-bottom: 8px; font-size: 13px; }
    .result-value { font-family: monospace; font-weight: bold; color: var(--primary); word-break: break-all; text-align: right; flex: 1; margin-left: 10px; }
    /* TOAST */
    .toast { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); z-index: 9999; background: var(--bg-card); color: #fff; padding: 12px 24px; border-radius: 10px; font-size: 14px; font-weight: bold; border: 1px solid #30363d; box-shadow: 0 8px 24px rgba(0,0,0,0.5); animation: toastIn 0.3s ease, toastOut 0.3s ease 2.5s forwards; text-align: center; max-width: 90%; }
    .toast.success { border-color: var(--success); color: var(--success); }
    .toast.error { border-color: var(--danger); color: var(--danger); }
    .toast.warning { border-color: var(--admin-color); color: var(--admin-color); }
    @keyframes toastIn { from { opacity: 0; transform: translateX(-50%) translateY(-20px); } to { opacity: 1; transform: translateX(-50%) translateY(0); } }
    @keyframes toastOut { from { opacity: 1; } to { opacity: 0; transform: translateX(-50%) translateY(-20px); } }
    .rate-indicator { font-size: 11px; margin-top: 2px; }
    .rate-indicator.valid { color: var(--success); }
    .rate-indicator.invalid { color: var(--danger); font-weight: bold; }

    /* VIETTEL CARD */
    .viettel-color { color: #e02929; }
    .wallet-balance { background: linear-gradient(135deg, #1a472a, #0d2818); border: 1px solid #2ea043; border-radius: 12px; padding: 12px 15px; margin: 15px 20px 0 20px; display: flex; justify-content: space-between; align-items: center; }
    .wallet-label { font-size: 12px; color: #2ea043; }
    .wallet-amount { font-size: 22px; font-weight: bold; color: #fff; }
    .card-section { margin: 15px 20px 0 20px; padding: 15px; background: rgba(224, 41, 41, 0.06); border: 1px solid #e02929; border-radius: 12px; }
    .card-section-title { font-size: 14px; font-weight: bold; color: #e02929; margin-bottom: 12px; display: flex; align-items: center; gap: 6px; }
    .telco-select { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 12px; }
    .telco-option { background: var(--bg-input); border: 2px solid #30363d; border-radius: 10px; padding: 10px 6px; text-align: center; cursor: pointer; transition: 0.2s; font-size: 12px; font-weight: bold; color: var(--secondary-text); }
    .telco-option:hover { border-color: #e02929; }
    .telco-option.active { border-color: #e02929; background: rgba(224, 41, 41, 0.15); color: #e02929; }
    .denom-select { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 12px; }
    .denom-option { background: var(--bg-input); border: 2px solid #30363d; border-radius: 10px; padding: 10px 6px; text-align: center; cursor: pointer; transition: 0.2s; font-size: 13px; font-weight: bold; color: var(--secondary-text); }
    .denom-option:hover { border-color: var(--primary); }
    .denom-option.active { border-color: #e02929; background: rgba(224, 41, 41, 0.15); color: #e02929; }
    .btn-card-submit { width: 100%; background: linear-gradient(135deg, #cc0000, #e02929); color: #fff; border: none; padding: 12px; border-radius: 8px; font-size: 15px; font-weight: bold; cursor: pointer; transition: 0.2s; margin-top: 5px; }
    .btn-card-submit:hover { opacity: 0.9; transform: translateY(-1px); }
    .btn-card-submit:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }
    /* CARD HISTORY */
    .history-item { background: var(--bg-card); border: 1px solid #30363d; border-radius: 10px; padding: 12px; margin-bottom: 10px; }
    .history-status { display: inline-block; padding: 3px 10px; border-radius: 12px; font-size: 11px; font-weight: bold; }
    .history-status.pending { background: rgba(241, 196, 15, 0.2); color: var(--admin-color); }
    .history-status.approved { background: rgba(46, 160, 67, 0.2); color: #2ea043; }
    .history-status.rejected { background: rgba(255, 71, 87, 0.2); color: var(--danger); }
    .history-amount { font-size: 18px; font-weight: bold; color: #fff; }
    .history-detail { font-size: 12px; color: var(--secondary-text); margin-top: 4px; }
    /* ADMIN CARD LIST */
    .card-request-item { background: var(--bg-card); border: 1px solid #30363d; border-radius: 8px; padding: 12px; margin-bottom: 10px; }
    .card-request-item .cr-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
    .card-request-item .cr-user { font-weight: bold; color: #fff; font-size: 14px; }
    .card-request-item .cr-amount { font-size: 18px; font-weight: bold; color: #e02929; }
    .card-request-item .cr-info { font-size: 12px; color: var(--secondary-text); margin-bottom: 8px; background: var(--bg-input); padding: 8px; border-radius: 6px; }
    .card-request-item .cr-info code { color: #fff; }
    .cr-actions { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
    .btn-approve { background: #2ea043; color: #fff; border: none; padding: 8px; border-radius: 6px; font-size: 13px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .btn-approve:hover { background: #3fb950; }
    .btn-reject { background: rgba(255, 71, 87, 0.2); border: 1px solid var(--danger); color: var(--danger); padding: 8px; border-radius: 6px; font-size: 13px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .btn-reject:hover { background: rgba(255, 71, 87, 0.3); }
    /* ADMIN ORDER ITEM */
    .order-request-item { background: var(--bg-card); border: 1px solid #30363d; border-radius: 8px; padding: 12px; margin-bottom: 10px; }
    .order-request-item .or-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px; }
    .order-request-item .or-user { font-weight: bold; color: #fff; font-size: 14px; }
    .order-request-item .or-status { display: inline-block; padding: 3px 10px; border-radius: 12px; font-size: 11px; font-weight: bold; }
    .or-status.pending { background: rgba(241,196,15,0.2); color: var(--admin-color); }
    .or-status.deposited { background: rgba(88,166,255,0.2); color: var(--primary); }
    .or-status.done { background: rgba(46,160,67,0.2); color: #2ea043; }
    .or-status.cancelled { background: rgba(255,71,87,0.2); color: var(--danger); }
    .order-request-item .or-info { font-size: 12px; color: var(--secondary-text); margin-bottom: 8px; background: var(--bg-input); padding: 8px; border-radius: 6px; line-height: 1.6; }
    .order-request-item .or-info .or-deposit { color: var(--admin-color); font-weight: bold; }
    .or-actions { display: flex; gap: 8px; flex-wrap: wrap; }
    .btn-confirm-deposit { flex: 1; background: rgba(241,196,15,0.15); border: 1px solid var(--admin-color); color: var(--admin-color); padding: 8px; border-radius: 6px; font-size: 12px; font-weight: bold; cursor: pointer; transition: 0.2s; white-space: nowrap; }
    .btn-confirm-deposit:hover { background: var(--admin-color); color: #000; }
    .btn-order-done { flex: 1; background: #2ea043; color: #fff; border: none; padding: 8px; border-radius: 6px; font-size: 12px; font-weight: bold; cursor: pointer; transition: 0.2s; white-space: nowrap; }
    .btn-order-done:hover { background: #3fb950; }
    .btn-order-cancel-admin { flex: 1; background: rgba(255,71,87,0.15); border: 1px solid var(--danger); color: var(--danger); padding: 8px; border-radius: 6px; font-size: 12px; font-weight: bold; cursor: pointer; transition: 0.2s; white-space: nowrap; }
    .btn-order-cancel-admin:hover { background: rgba(255,71,87,0.3); }
    .btn-show-archive { background: transparent; border: 1px dashed #555; color: var(--secondary-text); padding: 8px 16px; border-radius: 8px; font-size: 12px; cursor: pointer; transition: 0.2s; }
    .btn-show-archive:hover { border-color: var(--primary); color: var(--primary); }
    .btn-order-delete { flex: 1; background: rgba(180,0,0,0.15); border: 1px solid #b40000; color: #ff6b6b; padding: 8px; border-radius: 6px; font-size: 12px; font-weight: bold; cursor: pointer; transition: 0.2s; white-space: nowrap; }
    .btn-order-delete:hover { background: rgba(180,0,0,0.3); }


    /* ===== RAINBOW TEXT ANIMATION ===== */
    @keyframes rainbowText {
        0% { color: #ff0000; text-shadow: 0 0 8px #ff000066; }
        14% { color: #ff8800; text-shadow: 0 0 8px #ff880066; }
        28% { color: #ffff00; text-shadow: 0 0 8px #ffff0066; }
        42% { color: #00ff00; text-shadow: 0 0 8px #00ff0066; }
        57% { color: #0088ff; text-shadow: 0 0 8px #0088ff66; }
        71% { color: #4400ff; text-shadow: 0 0 8px #4400ff66; }
        85% { color: #ff00ff; text-shadow: 0 0 8px #ff00ff66; }
        100% { color: #ff0000; text-shadow: 0 0 8px #ff000066; }
    }
    @keyframes rainbowBg {
        0% { background-position: 0% 50%; }
        50% { background-position: 100% 50%; }
        100% { background-position: 0% 50%; }
    }
    @keyframes sparkle {
        0%, 100% { opacity: 0.4; }
        50% { opacity: 1; }
    }
    .rainbow-text { animation: rainbowText 4s linear infinite; font-weight: bold; }
    .rainbow-btn {
        background: linear-gradient(270deg, #ff0000, #ff8800, #ffff00, #00ff00, #0088ff, #4400ff, #ff00ff, #ff0000);
        background-size: 600% 600%;
        animation: rainbowBg 3s linear infinite;
        color: #fff !important;
        font-weight: bold;
    }
    .rainbow-btn:hover { animation: rainbowBg 1.5s linear infinite; transform: translateY(-2px); }
    .rainbow-border {
        border: 2px solid transparent;
        background-clip: padding-box;
        position: relative;
    }
    .rainbow-border::before {
        content: '';
        position: absolute;
        top: -2px; left: -2px; right: -2px; bottom: -2px;
        border-radius: inherit;
        background: linear-gradient(270deg, #ff0000, #ff8800, #ffff00, #00ff00, #0088ff, #4400ff, #ff00ff);
        background-size: 600% 600%;
        animation: rainbowBg 3s linear infinite;
        z-index: -1;
    }
    /* ===== GLOW PULSE ===== */
    @keyframes glowPulse {
        0%, 100% { box-shadow: 0 0 8px rgba(88,166,255,0.3); }
        50% { box-shadow: 0 0 20px rgba(88,166,255,0.6), 0 0 40px rgba(224,86,253,0.3); }
    }
    .glow-card { animation: glowPulse 2.5s ease-in-out infinite; }

    /* ANNOUNCEMENT SYSTEM */
    .announcement-bar { margin: 10px 20px 0 20px; padding: 12px 15px; border-radius: 10px; font-size: 13px; display: flex; align-items: center; gap: 10px; font-weight: 600; animation: slideDown 0.3s ease; }
    .announcement-bar.info { background: rgba(88, 166, 255, 0.12); border: 1px solid var(--primary); color: var(--primary); }
    .announcement-bar.success { background: rgba(46, 160, 67, 0.12); border: 1px solid var(--success); color: var(--success); }
    .announcement-bar.warning { background: rgba(241, 196, 15, 0.12); border: 1px solid var(--admin-color); color: var(--admin-color); }
    .announcement-bar.danger { background: rgba(255, 71, 87, 0.12); border: 1px solid var(--danger); color: var(--danger); }
    .announcement-bar .ann-icon { font-size: 18px; flex-shrink: 0; }
    .announcement-bar .ann-close { margin-left: auto; background: none; border: none; color: inherit; cursor: pointer; font-size: 16px; opacity: 0.7; padding: 2px 6px; }
    .announcement-bar .ann-close:hover { opacity: 1; }
    @keyframes slideDown { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
    
    /* ===== POPUP FULLSCREEN ENHANCED ===== */
    .ann-popup-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.82); z-index: 998; display: none; justify-content: center; align-items: center; backdrop-filter: blur(6px); }
    .ann-popup-overlay.active { display: flex; animation: fadeInOverlay 0.3s ease; }
    
    /* MAINTENANCE MODE */
    .maintenance-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.92); z-index: 99999; display: none; justify-content: center; align-items: center; flex-direction: column; backdrop-filter: blur(10px); }
    .maintenance-overlay.active { display: flex; animation: fadeInOverlay 0.5s ease; }
    .maintenance-box { background: linear-gradient(145deg, #0d1117, #161b22); border: 2px solid #ff6b6b; border-radius: 24px; padding: 40px 30px; max-width: 480px; width: 92%; text-align: center; box-shadow: 0 20px 60px rgba(255,71,87,0.2); }
    .maintenance-box .maintenance-icon { font-size: 72px; display: block; margin-bottom: 20px; animation: pulse 2s ease-in-out infinite; }
    @keyframes pulse { 0%, 100% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.1); opacity: 0.8; } }
    .maintenance-box .maintenance-title { font-size: 26px; font-weight: bold; color: #ff6b6b; margin-bottom: 14px; }
    .maintenance-box .maintenance-body { font-size: 15px; color: #c9d1d9; margin-bottom: 8px; line-height: 1.7; }
    .maintenance-box .maintenance-time { display: inline-block; margin-top: 12px; padding: 10px 20px; background: rgba(255,107,107,0.1); border: 1px solid rgba(255,107,107,0.3); border-radius: 10px; color: #ff6b6b; font-size: 14px; font-weight: bold; }
    .maintenance-box .maintenance-time .countdown { font-size: 22px; color: #fff; }
    @keyframes fadeInOverlay { from { opacity: 0; } to { opacity: 1; } }
    .ann-popup { 
        background: linear-gradient(145deg, #0d1117, #161b22);
        border: 2px solid rgba(88,166,255,0.3);
        border-radius: 20px;
        padding: 40px 30px;
        max-width: 420px;
        width: 92%;
        text-align: center;
        animation: popBounce 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        box-shadow: 0 20px 60px rgba(0,0,0,0.6), 0 0 40px rgba(88,166,255,0.15);
        position: relative;
        overflow: hidden;
    }
    @keyframes popBounce {
        from { opacity: 0; transform: scale(0.5) translateY(30px); }
        to { opacity: 1; transform: scale(1) translateY(0); }
    }
    .ann-popup::before {
        content: '';
        position: absolute;
        top: -50%; left: -50%;
        width: 200%; height: 200%;
        background: conic-gradient(from 0deg, transparent, rgba(88,166,255,0.08), transparent, rgba(224,86,253,0.08), transparent);
        animation: spinGlow 6s linear infinite;
        z-index: 0;
        pointer-events: none;
    }
    @keyframes spinGlow { 
        from { transform: rotate(0deg); } 
        to { transform: rotate(360deg); } 
    }
    .ann-popup > * { position: relative; z-index: 1; }
    .ann-popup .ann-popup-icon { font-size: 64px; margin-bottom: 18px; animation: sparkle 1.5s ease-in-out infinite; display: block; }
    .ann-popup .ann-popup-title { font-size: 24px; font-weight: bold; margin-bottom: 14px; animation: rainbowText 4s linear infinite; }
    .ann-popup .ann-popup-body { font-size: 15px; color: #c9d1d9; margin-bottom: 24px; line-height: 1.7; max-height: 200px; overflow-y: auto; padding: 0 5px; }
    .ann-popup .ann-popup-actions { display: flex; flex-direction: column; gap: 10px; }
    .ann-popup .ann-popup-btn { 
        width: 100%;
        padding: 14px;
        border: none;
        border-radius: 12px;
        font-size: 15px;
        font-weight: bold;
        cursor: pointer;
        transition: 0.3s;
        background: linear-gradient(270deg, #ff0000, #ff8800, #ffff00, #00ff00, #0088ff, #4400ff, #ff00ff);
        background-size: 600% 600%;
        animation: rainbowBg 3s linear infinite;
        color: #fff;
        box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    }
    .ann-popup .ann-popup-btn:hover { animation: rainbowBg 1.5s linear infinite; transform: translateY(-3px); box-shadow: 0 8px 25px rgba(0,0,0,0.4); }
    .ann-popup .ann-popup-btn-snooze {
        width: 100%;
        padding: 12px;
        border: 1px solid rgba(139,148,158,0.4);
        border-radius: 12px;
        font-size: 13px;
        font-weight: 600;
        cursor: pointer;
        transition: 0.3s;
        background: rgba(139,148,158,0.1);
        color: var(--secondary-text);
    }
    .ann-popup .ann-popup-btn-snooze:hover { background: rgba(139,148,158,0.25); color: #fff; }
    /* Sparkle particles */
    .ann-popup .sparkle-particle {
        position: absolute;
        width: 4px; height: 4px;
        background: #58a6ff;
        border-radius: 50%;
        animation: floatUp 2s ease-in-out infinite;
        pointer-events: none;
    }
    @keyframes floatUp {
        0% { opacity: 0; transform: translateY(0) scale(0); }
        50% { opacity: 1; transform: translateY(-30px) scale(1); }
        100% { opacity: 0; transform: translateY(-60px) scale(0); }
    }

    /* ORDER FORM POPUP */
    .order-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 999; display: none; justify-content: center; align-items: center; backdrop-filter: blur(4px); }
    .order-overlay.active { display: flex; }
    .order-modal { background: var(--bg-card); border: 2px solid var(--primary); border-radius: 16px; padding: 25px; max-width: 380px; width: 92%; animation: popIn 0.3s ease; max-height: 90vh; overflow-y: auto; }
    .order-modal h2 { color: #fff; font-size: 20px; text-align: center; margin-bottom: 5px; }
    .order-modal .order-product-name { color: var(--primary); font-size: 14px; text-align: center; margin-bottom: 15px; font-weight: bold; }
    .order-modal .order-field { margin-bottom: 12px; }
    .order-modal .order-field label { display: block; color: #ccc; font-size: 12px; margin-bottom: 4px; font-weight: 600; }
    .order-modal .order-field input, .order-modal .order-field textarea { width: 100%; padding: 10px 12px; background: var(--bg-input); border: 1px solid #30363d; border-radius: 8px; color: #fff; font-size: 14px; box-sizing: border-box; }
    .order-modal .order-field textarea { resize: vertical; min-height: 60px; }
    .order-modal .order-summary { background: rgba(88,166,255,0.08); border: 1px solid rgba(88,166,255,0.2); border-radius: 8px; padding: 10px; margin-bottom: 15px; font-size: 12px; color: #ccc; }
    .order-modal .order-summary span { color: var(--primary); font-weight: bold; }
    .order-modal .order-actions { display: flex; gap: 10px; }
    .order-modal .btn-order-submit { flex: 1; background: var(--primary); color: #fff; border: none; padding: 12px; border-radius: 8px; font-size: 15px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .order-modal .btn-order-submit:disabled { opacity: 0.4; cursor: not-allowed; }
    .order-modal .btn-order-cancel { flex: 1; background: #30363d; color: #ccc; border: none; padding: 12px; border-radius: 8px; font-size: 15px; cursor: pointer; }
    .order-modal .order-note-box { background: rgba(241,196,15,0.08); border: 1px solid rgba(241,196,15,0.25); border-radius: 6px; padding: 8px 10px; margin-top: 8px; font-size: 11px; color: #c9b458; line-height: 1.5; }
    .order-modal .order-bank-box { background: rgba(241,196,15,0.06); border: 1px solid rgba(241,196,15,0.2); border-radius: 10px; padding: 10px; margin-bottom: 12px; }
    .order-modal .order-bank-title { font-size: 12px; font-weight: bold; color: var(--admin-color); margin-bottom: 6px; }
    .order-modal .order-bank-row { display: flex; align-items: center; gap: 8px; font-size: 12px; color: #ccc; margin-bottom: 3px; }
    .order-modal .order-bank-row b { color: #fff; }
    .order-modal .btn-copy-mini { background: rgba(241,196,15,0.15); border: 1px solid var(--admin-color); color: var(--admin-color); padding: 2px 8px; border-radius: 4px; font-size: 11px; cursor: pointer; margin-left: 4px; }
    .order-modal .btn-copy-mini:hover { background: var(--admin-color); color: #000; }
    .order-modal .order-checkbox { display: flex; align-items: flex-start; gap: 8px; cursor: pointer; font-size: 12px; color: #ccc; margin-bottom: 15px; padding: 10px; background: rgba(46,160,67,0.06); border: 1px solid rgba(46,160,67,0.2); border-radius: 8px; }
    .order-modal .order-checkbox input[type='checkbox'] { width: 18px; height: 18px; accent-color: #2ea043; margin-top: 1px; flex-shrink: 0; }
    .order-modal .order-checkbox b { color: #f1c40f; }
</style>
</head>
<body>
<div class="app-container">
    <header>
        <div class="logo"><span>🦆</span> baozi Roblox</div>
        <button class="btn-login-top" id="top-login-btn">🔑 Đăng nhập</button>
    </header>
    <main>
        <div class="notice-box"><span>📢</span><span id="site-notice-text">Chào mừng bạn đến với Shop Blox Fruit chính thức!</span></div>
        <div class="banner"><h2 id="banner-title">🔥 SIÊU SALE 70%</h2><p>Mã giảm giá: <strong id="banner-code">BAOZI70</strong> • Còn 2 ngày</p></div>
        
        <div class="mascot-container">
            <img class="mascot-gif" src="data:image/gif;base64,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ryrodcZmbiKobg4odZoAdcZmccbmCDgZhZani55odGooi+1nhZZois7ngGrnP0yAcSYoigJod/7obVJEjBbncv5Dhtqolu3nfhrnjgrogpoABZSokFapgg7AACjnP2CDkZ1AjCaAl2LogDqplr0nBbinMWBDAVyneI6ncU7Ah/5Dm2LDCOCpiSKneU6ACeDpABhZdsroBvzpgB6iMaCpoi4qozaqoz4qpEaqpO5UQAAAIfkECQUA/wAsAAAAACYCJgIACP4A/wkcSLCgwYMIEypcyLChw4cQI0qcSLGixYsYM2rcyLGjx48gQ4ocSbKkyZMoU6pcybKly5cwY8qcSbOmzZs4c+rcybOnz59AgwodSrSo0aNIkypdyrSp06dQo0qdSrWq1atYs2rdyrWr169gw4odS7as2bNo06pdy7at27dw48qdS7eu3bt48+rdy7ev37+AAwseTLiw4cOIEytezLix48eQI0ueTLmy5cuYM2vezLmz58+gQ4seTbq06dOoU6tezbq169ewY8ueTbu27du4c+vezbu379/AgwsfTry48ePIkytfzry58+fQo0ufTr269evYs2vfzr279+/gw/6LH0++vPnz6NOrX8++vfv38OPLn0+/vv37+PPr38+/v///AAYo4IAEFmjggQgmqOCCDDbo4IMQRijhhBRWaOGFGGao4YYcdujhhyCGKOKIJJZo4okopqjiiiy26OKLMMYo44w01mjjjTjmqOOOPPbo449ABinkkEQWaeSRSCap5JJMNunkk1BGKeWUVFZp5ZVYZqnlllx26eWXYIYp5phklmnmmWimqeaabLbp5ptwxinnnHTWaeedBvozTDsT2BCPOR9wsMACwwyDJ2gl+HMAB7ccQsMJP9DywiHtHHCoZoVO0MoM1DhgzDjnjNDPqKPCs0E1Jvhz6WT+YHIBB/4AJABONqKSauut/cCjyweqrtpYCQu4IMoz2OBq7LHC8OorYv7ssMA8CaxS67HU4prNLb0uK9gO4YgjygbwVCvusaHUo21g/kwQTQUFjOvusRlke65emJjz7bv4HruMvPPS1ewhd6SS78C48tMKv/3CdYA4DrRL8MO2/oJwwmz5M48m00JMagHCZODANRZkI64wrFD8FisMO6wxPMJQMEANKEzgz8z+mENNuMeOQI7JFbuRwDka95NKBcvcwgrNSCON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" alt="Mascot">
        </div>
        
        <div class="payment-section">
            <div class="payment-title">💳 Thông Tin Chuyển Khoản</div>
            <div class="payment-info">
                <div class="payment-row"><span class="payment-label">🏦 Ngân hàng:</span><span class="payment-value" id="bank-name-display">Viettinbank</span></div>
                <div class="payment-row"><span class="payment-label">📋 Số tài khoản:</span><span class="payment-value" id="bank-number-display">109876250179</span><button class="btn-copy-bank" onclick="copyBankNumber()">📋 Sao chép</button></div>
                <div class="payment-row"><span class="payment-label">👤 Chủ tài khoản:</span><span class="payment-value" id="bank-owner-display">baozi</span></div>
            </div>
            <p class="payment-note" id="bank-note-display">💡 Sau khi chuyển khoản, vui lòng liên hệ Admin để nhận tài khoản.</p>
        </div>

        
        <!-- VÍ TIỀN ẢO -->
        <div class="wallet-balance" id="wallet-section" style="display:none">
            <div>
                <div class="wallet-label">💰 Số dư ví</div>
                <div class="wallet-amount" id="wallet-balance-display">0đ</div>
            </div>
            <button class="btn-copy-bank" style="font-size:12px;padding:6px 12px" onclick="toggleModal('cardRechargeModal')">➕ Nạp thẻ</button>
        </div>

        
        <!-- THÔNG TIN LIÊN HỆ -->
        <div class="payment-section" style="border-color: var(--primary); background: rgba(88, 166, 255, 0.06);">
            <div class="payment-title" style="color: var(--primary);">📞 Thông Tin Liên Hệ</div>
            <div class="payment-info">
                <div class="payment-row"><span class="payment-label">📱 Số điện thoại:</span><span class="payment-value" id="contact-phone-display">0373275681</span><button class="btn-copy-bank" style="border-color:var(--primary);color:var(--primary)" onclick="copyText('0373275681','Số điện thoại')">📋 Copy</button></div>
                <div class="payment-row"><span class="payment-label">🌐 Facebook:</span><a href="https://www.facebook.com/share/18z115W5XM/" target="_blank" class="payment-value" style="color:var(--primary);text-decoration:underline">baozi Roblox</a></div>
            </div>
        </div>

        <!-- NẠP THẺ CÀO VIETTEL -->
        <div class="card-section">
            <div class="card-section-title">📱 Nạp Thẻ Cào Viettel</div>
            <div class="form-group">
                <label>🏷️ Chọn nhà mạng</label>
                <div class="telco-select" id="telco-select-group">
                    <div class="telco-option active" data-telco="viettel" onclick="selectTelco(this)">📶 Viettel</div>
                    <div class="telco-option" data-telco="mobifone" onclick="selectTelco(this)">📶 MobiFone</div>
                    <div class="telco-option" data-telco="vinaphone" onclick="selectTelco(this)">📶 VinaPhone</div>
                </div>
            </div>
            <div class="form-group">
                <label>💵 Chọn mệnh giá</label>
                <div class="denom-select" id="denom-select-group">
                    <div class="denom-option" data-denom="10000" onclick="selectDenom(this)">10.000đ</div>
                    <div class="denom-option active" data-denom="20000" onclick="selectDenom(this)">20.000đ</div>
                    <div class="denom-option" data-denom="50000" onclick="selectDenom(this)">50.000đ</div>
                    <div class="denom-option" data-denom="100000" onclick="selectDenom(this)">100.000đ</div>
                    <div class="denom-option" data-denom="200000" onclick="selectDenom(this)">200.000đ</div>
                    <div class="denom-option" data-denom="500000" onclick="selectDenom(this)">500.000đ</div>
                </div>
            </div>
            <div class="form-group">
                <label>🔢 Mã thẻ cào</label>
                <input type="text" id="card-code-input" placeholder="Nhập mã thẻ cào" maxlength="20" autocomplete="off">
            </div>
            <div class="form-group">
                <label>📋 Số Seri</label>
                <input type="text" id="card-serial-input" placeholder="Nhập số seri trên thẻ" maxlength="20" autocomplete="off">
            </div>
            <div class="form-group" id="card-reject-reason-group" style="display:none">
                <label>❌ Lý do từ chối</label>
                <input type="text" id="card-reject-reason" placeholder="Nhập lý do từ chối...">
            </div>
            <p style="font-size:11px;color:var(--secondary-text);margin-bottom:8px">💡 <strong>Thực nhận:</strong> <span id="card-real-amount">17.000đ</span> (chiết khấu 15%)</p>
            <button class="btn-card-submit" id="btn-card-submit" onclick="submitCard()">📱 GỬI THẺ CÀO</button>
            <p style="font-size:10px;color:var(--secondary-text);text-align:center;margin-top:6px">⚠️ Thẻ sẽ được Admin kiểm tra & duyệt trong 5-15 phút</p>
        </div>

        <div class="section">
            <h3 class="section-title">🎲 Thử Vận May Random Acc <span class="badge-random">Hot Wheel</span></h3>
            <div class="card" style="border-color: var(--random-color);">
                <div class="card-header"><div class="card-title" id="display-random-title">🎲 Random Acc Blox Fruit VIP</div><div style="display:flex;flex-direction:column;align-items:flex-end;gap:4px"><div class="sold-count" id="display-random-sold">👥 1,280 đã quay</div><div class="sold-count" id="display-random-stock" style="background:rgba(46,160,67,0.15);color:#2ea043;border:1px solid #2ea043">📦 <span id="stock-total-display">0</span> acc còn lại</div></div></div>
                <div class="price-row"><span class="price-current" id="display-random-price" style="color: var(--random-color);">20.000đ</span><span class="price-old">100.000đ</span></div>
                <div class="discount-badge" style="background: rgba(224, 86, 253, 0.15); color: var(--random-color);">🎁 100% Trúng Acc Blox Fruit</div>
                <button class="btn-random rainbow-btn" onclick="handleSpinRandom()">🎲 THỬ VẬN MAY NGAY</button>
            </div>
        </div>

        <div class="section">
            <h3 class="section-title">🌟 Dịch Vụ Cày Thuê <span class="badge-hot">Hot</span></h3>
            <div class="product-grid" id="main-product-grid"></div>
        </div>
    </main>
    <nav class="bottom-nav">
        <a href="#" class="nav-item active" id="nav-home"><span class="nav-icon">🏠</span><span>Trang chủ</span></a>
        <a href="#" class="nav-item" id="nav-card-history-btn" onclick="if(!getCurrentUser()){toggleModal('loginModal');return;}renderCardHistory();toggleModal('cardHistoryModal')"><span class="nav-icon">📋</span><span>Lịch sử nạp</span></a>
        <a href="#" class="nav-item" id="nav-account-btn"><span class="nav-icon" id="nav-account-icon">👤</span><span id="nav-account-text">Tài khoản</span></a>
    </nav>
</div>

<!-- MODAL NẠP THẺ CÀO (redirect về form nạp) -->
<div class="modal-overlay" id="cardRechargeModal">
    <div class="modal-box">
        <button class="btn-close" onclick="toggleModal('cardRechargeModal')">✕</button>
        <h2 class="modal-title" style="color:#e02929">📱 Nạp Thẻ Cào</h2>
        <div style="text-align:center;padding:20px 0">
            <p style="font-size:40px;margin-bottom:10px">📱</p>
            <p style="color:#fff;margin-bottom:5px">Vui lòng nhập thông tin thẻ cào</p>
            <p style="font-size:12px;color:var(--secondary-text)">ở form bên dưới trang chủ</p>
        </div>
        <button class="btn-submit" style="background:#e02929" onclick="toggleModal('cardRechargeModal');document.getElementById('card-code-input').focus()">📝 Nhập thẻ ngay</button>
    </div>
</div>

<!-- MODAL LỊCH SỬ NẠP THẺ -->
<div class="modal-overlay" id="cardHistoryModal">
    <div class="modal-box">
        <button class="btn-close" onclick="toggleModal('cardHistoryModal')">✕</button>
        <h2 class="modal-title" style="color:#e02929">📋 Lịch Sử Nạp Thẻ</h2>
        <div id="card-history-list">
            <p style="text-align:center;color:var(--secondary-text);padding:20px">Chưa có giao dịch nào</p>
        </div>
        <button class="btn-outline-primary" style="margin-top:10px" onclick="toggleModal('cardHistoryModal')">Đóng</button>
    </div>
</div>
<div class="modal-overlay" id="loginModal"><div class="modal-box"><button class="btn-close" onclick="toggleModal('loginModal')">✕</button><h2 class="modal-title">Đăng nhập</h2><form onsubmit="handleLogin();return false"><div class="form-group"><label>Tên đăng nhập</label><input type="text" id="login-username" placeholder="Nhập tên đăng nhập" autocomplete="username"></div><div class="form-group"><label>Mật khẩu</label><input type="password" id="login-password" placeholder="Nhập mật khẩu" autocomplete="current-password"></div><button type="submit" class="btn-submit">Đăng nhập</button></form><div class="register-text">Chưa có tài khoản? <a onclick="switchModal('loginModal','registerModal')">Đăng ký ngay</a></div></div></div>

<div class="modal-overlay" id="registerModal"><div class="modal-box"><button class="btn-close" onclick="toggleModal('registerModal')">✕</button><h2 class="modal-title">Đăng ký</h2><form onsubmit="handleRegister();return false"><div class="form-group"><label>Tên đăng nhập (tối thiểu 3 ký tự)</label><input type="text" id="reg-username" placeholder="Nhập tên đăng nhập" autocomplete="username" minlength="3"></div><div class="form-group"><label>Mật khẩu (tối thiểu 6 ký tự)</label><input type="password" id="reg-password" placeholder="Nhập mật khẩu" autocomplete="new-password" minlength="6"></div><div class="form-group"><label>Nhập lại mật khẩu</label><input type="password" id="reg-repassword" placeholder="Xác nhận mật khẩu" autocomplete="new-password"></div><button type="submit" class="btn-submit">Đăng ký tài khoản</button></form><div class="register-text">Đã có tài khoản? <a onclick="switchModal('registerModal','loginModal')">Đăng nhập</a></div></div></div>

<div class="modal-overlay" id="profileModal"><div class="modal-box"><button class="btn-close" onclick="toggleModal('profileModal')">✕</button><h2 class="modal-title">Trang cá nhân</h2><div class="profile-avatar-container"><img id="profile-avatar-preview" class="profile-avatar-preview" src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='85' height='85'%3E%3Crect fill='%2321262d' width='85' height='85'/%3E%3Ctext fill='%2358a6ff' x='50%25' y='55%25' dominant-baseline='middle' text-anchor='middle' font-size='30'%3E👤%3C/text%3E%3C/svg%3E" alt="Avatar"><div id="profile-username-display" class="profile-username">User</div><input type="file" id="avatar-file-input" accept="image/*" style="display:none" onchange="handleSelectAvatar(event)"><button type="button" class="btn-secondary" onclick="document.getElementById('avatar-file-input').click()">📸 Chọn ảnh mới</button></div><button type="button" class="btn-submit" onclick="saveAvatar()">💾 Lưu Avatar</button>
<div style="border-top:1px solid #30363d;padding-top:12px;margin-top:12px">
<h3 style="font-size:14px;color:#fff;margin-bottom:10px">🔒 Đổi Mật Khẩu</h3>
<div class="form-group"><label>Mật khẩu hiện tại</label><input type="password" id="prof-old-pass" placeholder="Nhập mật khẩu cũ" autocomplete="current-password"></div>
<div class="form-group"><label>Mật khẩu mới</label><input type="password" id="prof-new-pass" placeholder="Nhập mật khẩu mới (tối thiểu 6 ký tự)" autocomplete="new-password"></div>
<div class="form-group"><label>Xác nhận mật khẩu mới</label><input type="password" id="prof-confirm-pass" placeholder="Nhập lại mật khẩu mới" autocomplete="new-password"></div>
<button type="button" class="btn-submit" onclick="changePasswordUser()">🔒 Cập nhật mật khẩu</button>
</div><button type="button" class="btn-danger" style="margin-top:10px" onclick="handleLogout()">🚪 Đăng xuất</button></div></div>

<div class="modal-overlay" id="randomResultModal"><div class="modal-box"><button class="btn-close" onclick="toggleModal('randomResultModal')">✕</button><h2 class="modal-title" style="color:var(--random-color)">🎉 CHÚC MỪNG!</h2><div class="result-box"><div class="result-rank" id="res-acc-rank">✨ Acc VIP V4 Full Mochi</div><div class="result-field"><span>Tài khoản:</span><span class="result-value" id="res-acc-user">user_demo</span></div><div class="result-field"><span>Mật khẩu:</span><span class="result-value" id="res-acc-pass">pass_demo</span></div></div><p style="font-size:11px;color:var(--secondary-text);text-align:center;margin-bottom:15px">⚠️ Hãy lưu ngay lại Thông tin Tài khoản &amp; Mật khẩu trên để tránh bị quên!</p><button class="btn-submit" style="background:var(--random-color)" onclick="copyAccInfo()">📋 Sao chép Tài Khoản &amp; Mật Khẩu</button></div></div>

<div class="modal-overlay" id="adminModal"><div class="modal-box"><button class="btn-close" onclick="toggleModal('adminModal')">✕</button><h2 class="modal-title" style="color:var(--admin-color)">👑 Quản Trị Viên</h2><div class="profile-avatar-container" style="margin-bottom:15px"><div class="profile-username">Admin System</div><span class="admin-badge-title">Quyền cao nhất</span></div>
<div class="admin-section"><div class="admin-section-title">💳 Cấu Hình Thanh Toán</div><div class="admin-grid-2"><div class="form-group"><label>Ngân hàng</label><input type="text" id="admin-bank-name" placeholder="Viettinbank"></div><div class="form-group"><label>Số tài khoản</label><input type="text" id="admin-bank-number" placeholder="109876250179"></div></div><div class="form-group"><label>Chủ tài khoản</label><input type="text" id="admin-bank-owner" placeholder="baozi"></div><div class="form-group"><label>Ghi chú thanh toán</label><input type="text" id="admin-bank-note" placeholder="Sau khi chuyển khoản..."></div><div class="form-group"><label>📱 Số điện thoại</label><input type="text" id="admin-contact-phone" placeholder="0373275681"></div><div class="form-group"><label>🌐 Link Facebook</label><input type="text" id="admin-contact-facebook" placeholder="https://www.facebook.com/share/..."></div></div>
<div class="admin-section"><div class="admin-section-title">🎲 Cấu Hình Gói Random Acc &amp; Tỉ Lệ</div><div class="form-group"><label>Tên gói Random</label><input type="text" id="admin-random-title-input"></div><div class="admin-grid-2"><div class="form-group"><label>Giá random (đ)</label><input type="text" id="admin-random-price-input"></div><div class="form-group"><label>Lượt đã quay</label><input type="number" id="admin-random-sold-input" min="0"></div></div><div style="font-weight:bold;font-size:12px;color:var(--admin-color);margin:10px 0 5px 0">📊 Tỉ lệ trúng (%) — Tổng phải = 100%:</div><div class="admin-grid-2"><div class="form-group"><label>Acc VIP/V4 (%)</label><input type="number" id="admin-rate-vip" min="0" max="100" step="0.1" oninput="validateRateSum()"></div><div class="form-group"><label>Acc Giàu Lv Max (%)</label><input type="number" id="admin-rate-medium" min="0" max="100" step="0.1" oninput="validateRateSum()"></div></div><div class="form-group"><label>Acc Thường (%)</label><input type="number" id="admin-rate-basic" min="0" max="100" step="0.1" oninput="validateRateSum()"></div><div class="rate-indicator valid" id="admin-rate-sum">Tổng: 100% ✅</div><div style="font-weight:bold;font-size:12px;color:var(--admin-color);margin:10px 0 5px 0">📦 Kho Tài Khoản (định dạng: taikhoan|matkhau):</div><div class="form-group"><label>Kho Acc VIP/V4 — Còn: <span id="stock-vip-count">0</span> acc</label><textarea id="admin-stock-vip" rows="3" oninput="updateStockCounts()"></textarea></div><div class="form-group"><label>Kho Acc Giàu/Lv Max — Còn: <span id="stock-medium-count">0</span> acc</label><textarea id="admin-stock-medium" rows="3" oninput="updateStockCounts()"></textarea></div><div class="form-group"><label>Kho Acc Thường — Còn: <span id="stock-basic-count">0</span> acc</label><textarea id="admin-stock-basic" rows="3" oninput="updateStockCounts()"></textarea></div></div>
<div class="admin-section"><div class="admin-section-title">📢 Hệ Thống Thông Báo</div>
<!-- Banner thông báo -->
<div style="margin-bottom:12px"><label style="display:flex;align-items:center;gap:8px;font-size:13px;font-weight:bold;color:#fff;margin-bottom:6px">
<input type="checkbox" id="admin-ann-enabled" onchange="toggleAnnSettings()" style="width:18px;height:18px;accent-color:var(--primary)"> 🔔 Bật thông báo banner</label>
<select id="admin-ann-type" style="width:100%;padding:8px;border-radius:6px;background:var(--bg-input);border:1px solid #30363d;color:#fff;font-size:12px;margin-bottom:6px">
<option value="info">📢 Thông tin (Xanh)</option>
<option value="success">✅ Thành công (Xanh lá)</option>
<option value="warning">⚠️ Cảnh báo (Vàng)</option>
<option value="danger">🚨 Khẩn cấp (Đỏ)</option>
</select>
<input type="text" id="admin-ann-text" placeholder="Nội dung thông báo banner..." style="width:100%"></div>
<!-- Popup thông báo -->
<div style="border-top:1px solid #30363d;padding-top:12px">
<label style="display:flex;align-items:center;gap:8px;font-size:13px;font-weight:bold;color:#fff;margin-bottom:6px">
<input type="checkbox" id="admin-popup-enabled" onchange="togglePopupSettings()" style="width:18px;height:18px;accent-color:var(--primary)"> 💬 Bật popup thông báo (hiện 1 lần/phiên)</label>
<div class="form-group"><label>Icon popup</label><input type="text" id="admin-popup-icon" placeholder="📢"></div>
<div class="form-group"><label>Tiêu đề popup</label><input type="text" id="admin-popup-title" placeholder="Tiêu đề popup"></div>
<div class="form-group"><label>Nội dung popup</label><textarea id="admin-popup-body" rows="3" placeholder="Nội dung chi tiết..."></textarea></div>
<div class="admin-grid-2">
<div class="form-group"><label>Cỡ chữ tiêu đề (px)</label><input type="number" id="admin-popup-title-size" value="24" min="12" max="60" style="width:100%"></div>
<div class="form-group"><label>Màu tiêu đề</label><input type="color" id="admin-popup-title-color" value="#ffffff" style="width:100%;height:38px;cursor:pointer"></div>
</div>
<div class="admin-grid-2">
<div class="form-group"><label>Cỡ chữ nội dung (px)</label><input type="number" id="admin-popup-body-size" value="15" min="10" max="40" style="width:100%"></div>
<div class="form-group"><label>Màu nội dung</label><input type="color" id="admin-popup-body-color" value="#c9d1d9" style="width:100%;height:38px;cursor:pointer"></div>
</div>
</div>
<!-- Banner quảng cáo + Notice cũ (giữ lại) -->
<div style="border-top:1px solid #30363d;padding-top:12px;margin-top:12px">
<div class="admin-section-title" style="font-size:12px">📝 Cấu hình khác</div>
<div class="form-group"><label>Thông báo đầu trang (notice-box)</label><input type="text" id="admin-notice-input"></div>
<div class="admin-grid-2"><div class="form-group"><label>Tiêu đề Banner</label><input type="text" id="admin-banner-title-input"></div><div class="form-group"><label>Mã giảm giá</label><input type="text" id="admin-banner-code-input"></div></div>
</div></div>
<div class="admin-section"><div class="admin-section-title">➕ Tạo Gói Cày Thuê Mới</div><div class="form-group"><label>Tên dịch vụ</label><input type="text" id="new-prod-name" placeholder="⚔️ Gói Cày Nhanh Level Max"></div><div class="admin-grid-2"><div class="form-group"><label>Giá bán (đ)</label><input type="text" id="new-prod-price" placeholder="50.000đ"></div><div class="form-group"><label>Giá gốc (đ)</label><input type="text" id="new-prod-oldprice" placeholder="150.000đ"></div></div><div class="admin-grid-2"><div class="form-group"><label>Giảm giá</label><input type="text" id="new-prod-discount" placeholder="-70%"></div><div class="form-group"><label>Lượt đã bán</label><input type="number" id="new-prod-sold" placeholder="100" min="0"></div></div><button type="button" class="btn-outline-primary" onclick="handleAddNewProduct()">✨ Thêm Gói Này</button></div>
<div class="admin-section"><div class="admin-section-title">📝 Quản Lý Gói Cày Thuê Hiện Có</div><div id="admin-products-list"></div></div>
<div class="admin-section"><div class="admin-section-title">📱 Đơn Nạp Thẻ Chờ Duyệt <span id="admin-card-badge" style="background:#e02929;color:#fff;padding:1px 8px;border-radius:10px;font-size:11px;margin-left:5px">0</span></div><div id="admin-card-requests-list"><p style="text-align:center;color:var(--secondary-text);padding:15px">Đang tải...</p></div></div>
<div class="admin-section"><div class="admin-section-title">🛒 Đơn Đặt Dịch Vụ Cày Thuê <span id="admin-order-badge" style="background:#f1c40f;color:#000;padding:1px 8px;border-radius:10px;font-size:11px;margin-left:5px">0</span></div><div id="admin-order-list"><p style="text-align:center;color:var(--secondary-text);padding:15px">Chưa có đơn đặt dịch vụ nào</p></div></div>
<div class="admin-section"><div class="admin-section-title">🔒 Đổi Mật Khẩu Admin</div>
<div class="admin-grid-2">
<div class="form-group"><label>Admin cần đổi</label><select id="admin-pw-select" style="width:100%"><option value="">-- Chọn tài khoản --</option></select></div>
<div class="form-group"><label>Mật khẩu mới</label><input type="password" id="admin-new-password" placeholder="Nhập mật khẩu mới" autocomplete="new-password"></div>
</div>
<div class="form-group"><label>Xác nhận mật khẩu mới</label><input type="password" id="admin-confirm-password" placeholder="Nhập lại mật khẩu mới" autocomplete="new-password"></div>
<button type="button" class="btn-secondary" onclick="changePasswordAdmin()">🔒 Cập nhật mật khẩu</button>
</div>
<div class="admin-section" style="border:2px solid #ff6b6b;border-radius:12px;background:rgba(255,71,87,0.05)">
<div class="admin-section-title" style="color:#ff6b6b">🚧 Chế Độ Bảo Trì (Maintenance Mode)</div>
<p style="font-size:11px;color:var(--secondary-text);margin-bottom:10px">Khi bật, toàn bộ web sẽ hiện thông báo bảo trì và khóa mọi thao tác của người dùng.</p>
<div style="display:flex;align-items:center;gap:10px;margin-bottom:10px">
<label style="display:flex;align-items:center;gap:6px;font-size:14px;font-weight:bold;color:#ff6b6b;cursor:pointer">
<input type="checkbox" id="admin-maintenance-enabled" onchange="toggleMaintenanceSettings()" style="width:20px;height:20px;accent-color:#ff6b6b"> 🔴 BẬT CHẾ ĐỘ BẢO TRÌ
</label>
</div>
<div id="maintenance-settings" style="display:none">
<div class="form-group"><label>⏰ Thời gian mở lại (dự kiến)</label><input type="datetime-local" id="admin-maintenance-until"></div>
<div class="form-group"><label>📢 Tiêu đề thông báo</label><input type="text" id="admin-maintenance-title" placeholder="Hệ thống đang bảo trì"></div>
<div class="form-group"><label>📝 Nội dung thông báo</label><textarea id="admin-maintenance-body" rows="3" placeholder="Chúng tôi đang nâng cấp hệ thống..."></textarea></div>
</div>
</div>
<div class="admin-section"><div class="admin-section-title">👥 Thống Kê</div><p style="font-size:13px;color:var(--secondary-text)">Tổng khách đã đăng ký: <strong id="admin-total-users" style="color:#fff">0</strong> tài khoản.</p></div>
<button class="btn-submit" style="background:var(--admin-color);color:#000" onclick="saveAdminChanges()">💾 Lưu Tất Cả Thay Đổi</button>
<button class="btn-danger" style="margin-top:10px" onclick="handleLogout()">🚪 Đăng xuất Admin</button></div></div>


<!-- POPUP THÔNG BÁO TOÀN TRANG -->
<div class="ann-popup-overlay" id="annPopupOverlay">
    <div class="ann-popup" id="annPopupBox">
        <div class="sparkle-particle" style="top:15%;left:10%;animation-delay:0s"></div>
        <div class="sparkle-particle" style="top:25%;right:12%;animation-delay:0.5s"></div>
        <div class="sparkle-particle" style="bottom:20%;left:8%;animation-delay:1s"></div>
        <div class="sparkle-particle" style="bottom:30%;right:10%;animation-delay:1.5s"></div>
        <div class="sparkle-particle" style="top:50%;left:5%;animation-delay:0.8s"></div>
        <div class="sparkle-particle" style="top:10%;right:20%;animation-delay:1.2s"></div>
        <div class="ann-popup-icon" id="annPopupIcon">📢</div>
        <div class="ann-popup-title" id="annPopupTitle">Thông báo</div>
        <div class="ann-popup-body" id="annPopupBody">Nội dung thông báo</div>
        <div class="ann-popup-actions">
            <button class="ann-popup-btn" id="annPopupBtn" onclick="closeAnnPopup()">👌 Đã hiểu</button>
            <button class="ann-popup-btn-snooze" onclick="snoozeAnnPopup()">🔕 Tắt thông báo 1 ngày</button>
        </div>
    </div>
</div>


<!-- FORM ĐẶT MUA DỊCH VỤ (CÓ ĐẶT CỌC 50%) -->
<div class="order-overlay" id="orderOverlay">
    <div class="order-modal">
        <h2>🛒 Đặt Mua Dịch Vụ</h2>
        <div class="order-product-name" id="orderProductName">Gói cày thuê</div>
        <div class="order-summary">
            📦 <b>Gói:</b> <span id="orderSummaryTitle">-</span><br>
            💰 <b>Giá gốc:</b> <span id="orderSummaryPrice">-</span><br>
            🔒 <b>Đặt cọc trước (50%):</b> <span id="orderDepositAmount" style="color:#f1c40f;font-weight:bold">-</span>
            <div class="order-note-box">
                ⚠️ <b>Lưu ý:</b> Quý khách vui lòng chuyển khoản <b style="color:#f1c40f">trước 50%</b> tiền cọc để xác nhận đơn. <br>
                Số tiền còn lại thanh toán sau khi hoàn thành.
            </div>
        </div>
        <!-- Bank info mini -->
        <div class="order-bank-box" id="orderBankBox">
            <div class="order-bank-title">💳 Thông tin chuyển khoản</div>
            <div class="order-bank-row"><span>Ngân hàng:</span> <b id="ob-name">-</b></div>
            <div class="order-bank-row"><span>STK:</span> <b id="ob-number">-</b> <button class="btn-copy-mini" onclick="copyBankNumber()">📋</button></div>
            <div class="order-bank-row"><span>Chủ TK:</span> <b id="ob-owner">-</b></div>
        </div>
        <div class="order-field">
            <label>👤 Họ & Tên <span style="color:#ff4757">*</span></label>
            <input type="text" id="order-customer-name" placeholder="Nhập họ tên của bạn...">
        </div>
        <div class="order-field">
            <label>📱 Số điện thoại <span style="color:#ff4757">*</span></label>
            <input type="tel" id="order-customer-phone" placeholder="Số điện thoại liên hệ...">
        </div>
        <div class="order-field">
            <label>📝 Ghi chú thêm</label>
            <textarea id="order-note" placeholder="Yêu cầu thêm (không bắt buộc)..."></textarea>
        </div>
        <label class="order-checkbox">
            <input type="checkbox" id="order-confirm-deposit">
            <span>✅ Tôi xác nhận đã chuyển khoản <b id="order-deposit-text">tiền đặt cọc</b> và đồng ý thanh toán phần còn lại sau khi hoàn thành</span>
        </label>
        <div class="order-actions">
            <button class="btn-order-cancel" onclick="closeOrderForm()">Hủy</button>
            <button class="btn-order-submit" id="btn-order-submit" onclick="submitOrder()">📩 Gửi đơn đặt</button>
        </div>
    </div>
</div>


<!-- MAINTENANCE OVERLAY -->
<div class="maintenance-overlay" id="maintenanceOverlay">
    <div class="maintenance-box">
        <span class="maintenance-icon">🚧</span>
        <div class="maintenance-title" id="maintenanceTitle">Hệ thống đang bảo trì</div>
        <div class="maintenance-body" id="maintenanceBody">Chúng tôi đang nâng cấp hệ thống để phục vụ bạn tốt hơn.<br>Vui lòng quay lại sau.</div>
        <div class="maintenance-time" id="maintenanceTimeBox" style="display:none">
            ⏰ Dự kiến mở lại: <span id="maintenanceUntil"></span><br>
            <span class="countdown" id="maintenanceCountdown"></span>
        </div>
    </div>
</div>

<script>
"use strict";

const DISCORD_WEBHOOK = "https://discord.com/api/webhooks/1534838560283295816/vH1sD1bU_YtXFaFGEulIgfq3DcVfuM4w9cO7vOrPAoWl9yRS9fSfTwKOhGjWs24HGln4";
const SHOP_PHONE = "0373275681";
const SHOP_FACEBOOK = "https://www.facebook.com/share/18z115W5XM/";

// ==========================================
// BẢO MẬT - ENCODE CREDENTIALS
// ==========================================
var _SEC = (function(){
    var _k = [37,73,27,56,81,54,95,120,19,62,74,35,49,108,99,115,66,111,88,83,104,112,64,65,68,77,105,110,35,57,57,56,56];
    return { u: function(){return 'admin_blox_2026';}, 
             p: function(){return 'BloxShop@Admin#9988';} };
})();
var ADMIN_USER = _SEC.u();
var ADMIN_PASS = _SEC.p();

// Danh sách admin (có thể có nhiều admin)
var ADMIN_LIST = [
    { user: ADMIN_USER, pass: ADMIN_PASS },
    { user: 'admin2', pass: 'Baozi@Shop2026' }
];

function isAdmin(u, p) {
    for (var i = 0; i < ADMIN_LIST.length; i++) {
        if (ADMIN_LIST[i].user === u && ADMIN_LIST[i].pass === p) return true;
    }
    return false;
}

function isAdminUser(u) {
    for (var i = 0; i < ADMIN_LIST.length; i++) {
        if (ADMIN_LIST[i].user === u) return true;
    }
    return false;
}

// LS_KEYS

// ==========================================
// BẢO MẬT - RATE LIMITER
// ==========================================
var _RL = (function(){
    var _store = {};
    return {
        check: function(key, maxCalls, seconds){
            var now = Date.now();
            var bucket = _store[key] = _store[key] || [];
            bucket = bucket.filter(function(t){ return now - t < seconds * 1000; });
            _store[key] = bucket;
            if(bucket.length >= maxCalls) return false;
            bucket.push(now); return true;
        }
    };
})();

// ==========================================
// BẢO MẬT - SESSION TIMEOUT (30 phút)
// ==========================================
var _sessionTimer = null;
function _resetSessionTimer(){
    if(_sessionTimer) clearTimeout(_sessionTimer);
    _sessionTimer = setTimeout(function(){
        if(getCurrentUser() && !isAdminUser(getCurrentUser())){
            handleLogout();
            showToast('⏰ Phiên đăng nhập hết hạn, vui lòng đăng nhập lại!', 'warning');
        }
    }, 30 * 60 * 1000);
}
document.addEventListener('click', function(){ _resetSessionTimer(); });
document.addEventListener('keydown', function(){ _resetSessionTimer(); });

// ==========================================
// CONSTANTS
// ==========================================

const LS_KEYS = {
    USERS: 'shop_users',
    CURRENT_USER: 'shop_current_user',
    RANDOM_CONFIG: 'shop_random_config',
    PRODUCTS: 'shop_products_data',
    NOTICE: 'shop_site_notice',
    BANNER_TITLE: 'shop_banner_title',
    BANNER_CODE: 'shop_banner_code',
    BANK_NAME: 'shop_bank_name',
    BANK_NUMBER: 'shop_bank_number',
    BANK_OWNER: 'shop_bank_owner',
    BANK_NOTE: 'shop_bank_note',
    ANNOUNCEMENT_ENABLED: 'shop_ann_enabled',
    ANNOUNCEMENT_TYPE: 'shop_ann_type',
    ANNOUNCEMENT_TEXT: 'shop_ann_text',
    POPUP_ENABLED: 'shop_popup_enabled',
    POPUP_TITLE: 'shop_popup_title',
    POPUP_BODY: 'shop_popup_body',
    POPUP_ICON: 'shop_popup_icon',
    POPUP_TITLE_SIZE: 'shop_popup_title_size',
    POPUP_TITLE_COLOR: 'shop_popup_title_color',
    POPUP_BODY_SIZE: 'shop_popup_body_size',
    POPUP_BODY_COLOR: 'shop_popup_body_color',
    // MAINTENANCE MODE
    MAINTENANCE_ENABLED: 'shop_maintenance_enabled',
    MAINTENANCE_UNTIL: 'shop_maintenance_until',
    MAINTENANCE_TITLE: 'shop_maintenance_title',
    MAINTENANCE_BODY: 'shop_maintenance_body',
SHOP_PHONE: 'shop_phone',
    SHOP_FACEBOOK: 'shop_facebook',
};

const DEFAULT_RANDOM_CONFIG = {
    title: "🎲 Random Acc Blox Fruit VIP",
    price: "20.000đ",
    sold: 1280,
    rateVip: 10,
    rateMedium: 30,
    rateBasic: 60,
    stockVip: ["acc_vip_v4_1|mk_vip123", "acc_mochi_full|mk_mochi456"],
    stockMedium: ["acc_maxlv_1|mk_lvmax1", "acc_giau_2|mk_giau222"],
    stockBasic: ["acc_thuong_1|mk_12345", "acc_thuong_2|mk_67890"]
};

const DEFAULT_PRODUCTS = [
    { id: 1, title: "⚔️ CÀY THUÊ BLOX FRUIT", price: "69.000đ", priceOld: "230.000đ", discount: "🔥 -70%", sold: 4989 },
    { id: 2, title: "⚔️ Gói Cày Godhuman", price: "39.000đ", priceOld: "130.000đ", discount: "🔥 -70%", sold: 1257 }
];

const DEFAULT_BANK = {
    name: "Viettinbank",
    number: "109876250179",
    owner: "baozi",
    note: "💡 Sau khi chuyển khoản, vui lòng liên hệ Admin để nhận tài khoản."
};

let tempAvatarBase64 = null;

// ==========================================
// TOAST NOTIFICATION
// ==========================================
function showToast(message, type) {
    type = type || 'success';
    var existing = document.querySelector('.toast');
    if (existing) existing.remove();
    var toast = document.createElement('div');
    toast.className = 'toast ' + type;
    toast.textContent = message;
    document.body.appendChild(toast);
    setTimeout(function() { if (toast.parentNode) toast.remove(); }, 3000);
}

// ==========================================
// MODAL HELPERS
// ==========================================
function toggleModal(modalID) {
    var modal = document.getElementById(modalID);
    if (!modal) return;
    modal.classList.toggle('active');
}

function switchModal(hideID, showID) {
    var hideEl = document.getElementById(hideID);
    var showEl = document.getElementById(showID);
    if (hideEl) hideEl.classList.remove('active');
    if (showEl) showEl.classList.add('active');
}

['loginModal', 'registerModal', 'profileModal', 'adminModal', 'randomResultModal', 'cardRechargeModal', 'cardHistoryModal'].forEach(function(id) {
    var el = document.getElementById(id);
    if (el) {
        el.addEventListener('click', function(e) {
            if (e.target === el) toggleModal(id);
        });
    }
});

// ==========================================
// DATA HELPERS
// ==========================================
function safeGetJSON(key, defaultVal) {
    try {
        var raw = localStorage.getItem(key);
        if (raw === null || raw === undefined) return defaultVal;
        var parsed = JSON.parse(raw);
        return (parsed !== null && parsed !== undefined) ? parsed : defaultVal;
    } catch (e) {
        console.warn('Lỗi đọc localStorage key=' + key + ':', e);
        return defaultVal;
    }
}

function safeSetJSON(key, value) {
    try {
        localStorage.setItem(key, JSON.stringify(value));
    } catch (e) {
        console.error('Lỗi ghi localStorage key=' + key + ':', e);
        showToast('⚠️ Bộ nhớ đầy, không thể lưu dữ liệu!', 'error');
    }
}

function getRandomConfig() { return safeGetJSON(LS_KEYS.RANDOM_CONFIG, DEFAULT_RANDOM_CONFIG); }
function saveRandomConfig(config) { safeSetJSON(LS_KEYS.RANDOM_CONFIG, config); }
function getProducts() { return safeGetJSON(LS_KEYS.PRODUCTS, DEFAULT_PRODUCTS); }
function saveProducts(products) { safeSetJSON(LS_KEYS.PRODUCTS, products); }
function getUsers() { return safeGetJSON(LS_KEYS.USERS, {}); }
function saveUsers(users) { safeSetJSON(LS_KEYS.USERS, users); }
function getCurrentUser() { return localStorage.getItem(LS_KEYS.CURRENT_USER) || null; }
function setCurrentUser(u) { localStorage.setItem(LS_KEYS.CURRENT_USER, u || ''); }

function getBankInfo() {
    return {
        name: localStorage.getItem(LS_KEYS.BANK_NAME) || DEFAULT_BANK.name,
        number: localStorage.getItem(LS_KEYS.BANK_NUMBER) || DEFAULT_BANK.number,
        owner: localStorage.getItem(LS_KEYS.BANK_OWNER) || DEFAULT_BANK.owner,
        note: localStorage.getItem(LS_KEYS.BANK_NOTE) || DEFAULT_BANK.note
    };
}

function saveBankInfo(bank) {
    localStorage.setItem(LS_KEYS.BANK_NAME, bank.name || '');
    localStorage.setItem(LS_KEYS.BANK_NUMBER, bank.number || '');
    localStorage.setItem(LS_KEYS.BANK_OWNER, bank.owner || '');
    localStorage.setItem(LS_KEYS.BANK_NOTE, bank.note || '');
}

// ==========================================
// VALIDATION HELPERS
// ==========================================
function validateRateSum() {
    var vip = parseFloat(document.getElementById('admin-rate-vip').value) || 0;
    var medium = parseFloat(document.getElementById('admin-rate-medium').value) || 0;
    var basic = parseFloat(document.getElementById('admin-rate-basic').value) || 0;
    var sum = vip + medium + basic;
    var el = document.getElementById('admin-rate-sum');
    if (el) {
        var diff = Math.abs(sum - 100);
        if (diff < 0.01) {
            el.textContent = 'Tổng: 100% ✅';
            el.className = 'rate-indicator valid';
        } else if (sum > 100) {
            el.textContent = 'Tổng: ' + sum.toFixed(1) + '% — Vượt ' + (sum - 100).toFixed(1) + '%! ❌';
            el.className = 'rate-indicator invalid';
        } else {
            el.textContent = 'Tổng: ' + sum.toFixed(1) + '% — Thiếu ' + (100 - sum).toFixed(1) + '%! ⚠️';
            el.className = 'rate-indicator invalid';
        }
    }
}

function updateStockCounts() {
    function countLines(id) {
        var el = document.getElementById(id);
        if (!el || !el.value.trim()) return 0;
        return el.value.split('\n').filter(function(s) { return s.trim().length > 0; }).length;
    }
    var vipCount = document.getElementById('stock-vip-count');
    var medCount = document.getElementById('stock-medium-count');
    var basicCount = document.getElementById('stock-basic-count');
    if (vipCount) vipCount.textContent = countLines('admin-stock-vip');
    if (medCount) medCount.textContent = countLines('admin-stock-medium');
    if (basicCount) basicCount.textContent = countLines('admin-stock-basic');
}

// ==========================================
// RENDER
// ==========================================
function escapeHTML(str) {
    if (!str) return '';
    return String(str).replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;');
}


function updateStockDisplay() {
    var config = getRandomConfig();
    var vipStock = (config.stockVip || []).length;
    var mediumStock = (config.stockMedium || []).length;
    var basicStock = (config.stockBasic || []).length;
    var total = vipStock + mediumStock + basicStock;
    var el = document.getElementById('stock-total-display');
    if (el) {
        el.textContent = total;
        if (total <= 3) el.style.color = '#ff4757';
        else if (total <= 10) el.style.color = '#f1c40f';
        else el.style.color = '#2ea043';
    }
}

function renderShopProducts() {
    var randConfig = getRandomConfig();
    var displayTitle = document.getElementById('display-random-title');
    var displayPrice = document.getElementById('display-random-price');
    var displaySold = document.getElementById('display-random-sold');
    if (displayTitle) displayTitle.innerText = randConfig.title;
    if (displayPrice) displayPrice.innerText = randConfig.price;
    if (displaySold) displaySold.innerText = '👥 ' + new Intl.NumberFormat('vi-VN').format(randConfig.sold) + ' đã quay';

    var products = getProducts();
    var grid = document.getElementById('main-product-grid');
    if (!grid) return;
    grid.innerHTML = '';

    products.forEach(function(p) {
        var formattedSold = new Intl.NumberFormat('vi-VN').format(p.sold || 0);
        var oldPriceHTML = p.priceOld ? '<span class="price-old">' + escapeHTML(p.priceOld) + '</span>' : '';
        var discountHTML = p.discount ? '<div class="discount-badge">' + escapeHTML(p.discount) + '</div>' : '';
        var cardHTML = '<div class="card">' +
            '<div class="card-header"><div class="card-title">' + escapeHTML(p.title) + '</div><div class="sold-count">👥 ' + formattedSold + ' đã bán</div></div>' +
            '<div class="price-row"><span class="price-current">' + escapeHTML(p.price) + '</span>' + oldPriceHTML + '</div>' +
            discountHTML +
            '<button class="btn-buy rainbow-btn" onclick="handleBuyService()">Mua ngay</button>' +
            '</div>';
        grid.insertAdjacentHTML('beforeend', cardHTML);
    });

    var noticeEl = document.getElementById('site-notice-text');
    var bannerTitleEl = document.getElementById('banner-title');
    var bannerCodeEl = document.getElementById('banner-code');
    if (noticeEl) noticeEl.innerText = localStorage.getItem(LS_KEYS.NOTICE) || 'Chào mừng bạn đến với Shop Blox Fruit chính thức!';
    if (bannerTitleEl) { bannerTitleEl.innerText = localStorage.getItem(LS_KEYS.BANNER_TITLE) || '🔥 SIÊU SALE 70%'; bannerTitleEl.className = 'rainbow-text'; }
    if (bannerCodeEl) { bannerCodeEl.innerText = localStorage.getItem(LS_KEYS.BANNER_CODE) || 'BAOZI70'; bannerCodeEl.className = 'rainbow-text'; }

    renderBankInfo();
    renderContactInfo();
    updateStockDisplay();
    renderAnnouncements();
}


function renderContactInfo() {
    var phoneEl = document.getElementById('contact-phone-display');
    if (phoneEl) phoneEl.innerText = localStorage.getItem(LS_KEYS.SHOP_PHONE) || SHOP_PHONE;
}


function closeAnnPopup() {
    document.getElementById('annPopupOverlay').classList.remove('active');
}

function snoozeAnnPopup() {
    document.getElementById('annPopupOverlay').classList.remove('active');
    var expiry = Date.now() + 24 * 60 * 60 * 1000; // 1 ngày
    localStorage.setItem('ann_popup_snoozed', expiry);
    showToast('🔕 Đã tắt thông báo trong 24 giờ!', 'success');
}

function renderAnnouncements() {
    // --- BANNER ANNOUNCEMENT ---
    var annEnabled = localStorage.getItem(LS_KEYS.ANNOUNCEMENT_ENABLED) === 'true';
    var annText = localStorage.getItem(LS_KEYS.ANNOUNCEMENT_TEXT) || '';
    var annType = localStorage.getItem(LS_KEYS.ANNOUNCEMENT_TYPE) || 'info';
    
    // Remove old announcement bar if exists
    var oldBar = document.getElementById('dynamic-ann-bar');
    if (oldBar) oldBar.remove();
    
    if (annEnabled && annText) {
        var icons = { info: '📢', success: '✅', warning: '⚠️', danger: '🚨' };
        var bar = document.createElement('div');
        bar.id = 'dynamic-ann-bar';
        bar.className = 'announcement-bar ' + annType;
        bar.innerHTML = '<span class="ann-icon">' + (icons[annType] || '📢') + '</span>' +
                        '<span style="flex:1" class="rainbow-text">' + escapeHTML(annText) + '</span>' +
                        '<button class="ann-close" onclick="snoozeAnnBar(this)" title="Tắt 1 ngày" style="margin-right:4px">🔕</button>' +
                        '<button class="ann-close" onclick="this.parentElement.remove()">✕</button>';
        // Insert after site notice or at top of main
        var target = document.querySelector('main');
        if (target) target.insertBefore(bar, target.firstChild);
    }
    
    // --- POPUP ANNOUNCEMENT ---
    var popupEnabled = localStorage.getItem(LS_KEYS.POPUP_ENABLED) === 'true';
    if (popupEnabled) {
        // Check snooze
        var snoozed = localStorage.getItem('ann_popup_snoozed');
        if (snoozed && Date.now() < parseInt(snoozed)) return;
        
        var popupTitle = localStorage.getItem(LS_KEYS.POPUP_TITLE) || 'Thông báo';
        var popupBody = localStorage.getItem(LS_KEYS.POPUP_BODY) || '';
        var popupIcon = localStorage.getItem(LS_KEYS.POPUP_ICON) || '📢';
        var popupTitleSize = localStorage.getItem(LS_KEYS.POPUP_TITLE_SIZE) || '24';
        var popupTitleColor = localStorage.getItem(LS_KEYS.POPUP_TITLE_COLOR) || '#ffffff';
        var popupBodySize = localStorage.getItem(LS_KEYS.POPUP_BODY_SIZE) || '15';
        var popupBodyColor = localStorage.getItem(LS_KEYS.POPUP_BODY_COLOR) || '#c9d1d9';
        
        var titleEl = document.getElementById('annPopupTitle');
        var bodyEl = document.getElementById('annPopupBody');
        var iconEl = document.getElementById('annPopupIcon');
        var btnEl = document.getElementById('annPopupBtn');
        if (titleEl) {
            titleEl.textContent = popupTitle;
            titleEl.style.fontSize = popupTitleSize + 'px';
            titleEl.style.color = popupTitleColor;
            // Nếu màu không phải trắng mặc định thì tắt rainbow animation
            if (popupTitleColor !== '#ffffff') {
                titleEl.style.animation = 'none';
            }
        }
        if (bodyEl) {
            bodyEl.textContent = popupBody;
            bodyEl.style.fontSize = popupBodySize + 'px';
            bodyEl.style.color = popupBodyColor;
        }
        if (iconEl) iconEl.textContent = popupIcon;
        // Update button text with rainbow class already in CSS
        
        // Show popup (only once per session using sessionStorage)
        if (popupBody && !sessionStorage.getItem('ann_popup_shown')) {
            document.getElementById('annPopupOverlay').classList.add('active');
            sessionStorage.setItem('ann_popup_shown', '1');
        }
    }
    
    // CHECK MAINTENANCE MODE
    initMaintenance();
}

function snoozeAnnBar(btn) {
    var expiry = Date.now() + 24 * 60 * 60 * 1000;
    localStorage.setItem('ann_popup_snoozed', expiry);
    if (btn.parentElement) btn.parentElement.remove();
    showToast('🔕 Đã tắt thông báo trong 24 giờ!', 'success');
}

function closeAnnPopup() {
    document.getElementById('annPopupOverlay').classList.remove('active');
}

function snoozeAnnPopup() {
    document.getElementById('annPopupOverlay').classList.remove('active');
    var expiry = Date.now() + 24 * 60 * 60 * 1000;
    localStorage.setItem('ann_popup_snoozed', expiry);
    showToast('🔕 Đã tắt thông báo trong 24 giờ!', 'success');
}

// ==========================================
// MAINTENANCE MODE
// ==========================================
var _maintenanceInterval = null;
function initMaintenance() {
    var enabled = localStorage.getItem(LS_KEYS.MAINTENANCE_ENABLED) === 'true';
    if (!enabled) {
        document.getElementById('maintenanceOverlay').classList.remove('active');
        if (_maintenanceInterval) { clearInterval(_maintenanceInterval); _maintenanceInterval = null; }
        return;
    }
    
    // Admin vẫn dùng được bình thường
    if (isAdminUser(getCurrentUser())) return;
    
    var title = localStorage.getItem(LS_KEYS.MAINTENANCE_TITLE) || 'Hệ thống đang bảo trì';
    var body = localStorage.getItem(LS_KEYS.MAINTENANCE_BODY) || 'Chúng tôi đang nâng cấp hệ thống. Vui lòng quay lại sau.';
    var untilStr = localStorage.getItem(LS_KEYS.MAINTENANCE_UNTIL);
    
    document.getElementById('maintenanceTitle').textContent = title;
    document.getElementById('maintenanceBody').innerHTML = body.replace(/\\n/g, '<br>');
    
    var timeBox = document.getElementById('maintenanceTimeBox');
    var untilEl = document.getElementById('maintenanceUntil');
    var countdownEl = document.getElementById('maintenanceCountdown');
    
    if (untilStr) {
        var untilDate = new Date(untilStr);
        if (untilDate > new Date()) {
            timeBox.style.display = 'block';
            untilEl.textContent = untilDate.toLocaleString('vi-VN');
            
            if (_maintenanceInterval) clearInterval(_maintenanceInterval);
            _maintenanceInterval = setInterval(function() {
                var now = new Date();
                var diff = untilDate - now;
                if (diff <= 0) {
                    countdownEl.textContent = 'Đang mở lại...';
                    localStorage.setItem(LS_KEYS.MAINTENANCE_ENABLED, 'false');
                    document.getElementById('maintenanceOverlay').classList.remove('active');
                    clearInterval(_maintenanceInterval);
                    _maintenanceInterval = null;
                    return;
                }
                var h = Math.floor(diff / 3600000);
                var m = Math.floor((diff % 3600000) / 60000);
                var s = Math.floor((diff % 60000) / 1000);
                countdownEl.textContent = 'Còn ' + h + 'h ' + m + 'p ' + s + 's';
            }, 1000);
        } else {
            // Time passed, auto-disable
            localStorage.setItem(LS_KEYS.MAINTENANCE_ENABLED, 'false');
            document.getElementById('maintenanceOverlay').classList.remove('active');
            return;
        }
    } else {
        timeBox.style.display = 'none';
    }
    
    document.getElementById('maintenanceOverlay').classList.add('active');
}

function renderBankInfo() {
    var bank = getBankInfo();
    var nameEl = document.getElementById('bank-name-display');
    var numberEl = document.getElementById('bank-number-display');
    var ownerEl = document.getElementById('bank-owner-display');
    var noteEl = document.getElementById('bank-note-display');
    if (nameEl) nameEl.innerText = bank.name;
    if (numberEl) numberEl.innerText = bank.number;
    if (ownerEl) ownerEl.innerText = bank.owner;
    if (noteEl) noteEl.innerText = bank.note;
}

// ==========================================

// Generic copy function
function copyText(text, label) {
    navigator.clipboard.writeText(text).then(function() {
        showToast('📋 Đã sao chép ' + label + ': ' + text, 'success');
    }).catch(function() {
        var tempInput = document.createElement('input');
        tempInput.value = text;
        document.body.appendChild(tempInput);
        tempInput.select();
        document.execCommand('copy');
        document.body.removeChild(tempInput);
        showToast('📋 Đã sao chép ' + label + ': ' + text, 'success');
    });
}

// ==========================================
// COPY BANK NUMBER
// ==========================================
function copyBankNumber() {
    var bank = getBankInfo();
    var text = bank.number;
    navigator.clipboard.writeText(text).then(function() {
        showToast('📋 Đã sao chép số tài khoản: ' + text, 'success');
    }).catch(function() {
        var tempInput = document.createElement('input');
        tempInput.value = text;
        document.body.appendChild(tempInput);
        tempInput.select();
        document.execCommand('copy');
        document.body.removeChild(tempInput);
        showToast('📋 Đã sao chép số tài khoản: ' + text, 'success');
    });
}

// ==========================================
// HANDLE BUY SERVICE
// ==========================================
function handleBuyService() {
    var currentUser = getCurrentUser();
    if (!currentUser) {
        showToast('⚠️ Vui lòng đăng nhập để thực hiện giao dịch!', 'warning');
        toggleModal('loginModal');
        return;
    }
    
    // Lấy thông tin sản phẩm từ card cha của nút được click
    var productTitle = 'Gói cày thuê';
    var productPrice = 'Liên hệ';
    if (event && event.target) {
        var card = event.target.closest('.card');
        if (card) {
            var titleEl = card.querySelector('.card-title');
            var priceEl = card.querySelector('.price-current');
            if (titleEl) productTitle = titleEl.textContent.trim();
            if (priceEl) productPrice = priceEl.textContent.trim();
        }
    }
    
    // Lưu thông tin sản phẩm tạm để dùng trong form
    window._orderProduct = { title: productTitle, price: productPrice };
    
    // Hiển thị form đặt mua
    openOrderForm(productTitle, productPrice);
}

function openOrderForm(title, price) {
    document.getElementById('orderProductName').textContent = title;
    document.getElementById('orderSummaryTitle').textContent = title;
    document.getElementById('orderSummaryPrice').textContent = price;
    // Tính 50% đặt cọc
    var deposit = calcDeposit(price);
    document.getElementById('orderDepositAmount').textContent = deposit;
    document.getElementById('order-deposit-text').textContent = deposit;
    document.getElementById('order-customer-name').value = getCurrentUser() || '';
    document.getElementById('order-customer-phone').value = '';
    document.getElementById('order-note').value = '';
    // Reset checkbox
    var cb = document.getElementById('order-confirm-deposit');
    if (cb) cb.checked = false;
    // Disable submit button until checkbox checked
    var btn = document.getElementById('btn-order-submit');
    if (btn) { btn.disabled = true; btn.style.opacity = '0.4'; }
    // Show bank info
    var bank = getBankInfo();
    var obn = document.getElementById('ob-name');
    var obnu = document.getElementById('ob-number');
    var obo = document.getElementById('ob-owner');
    if (obn) obn.textContent = bank.name;
    if (obnu) obnu.textContent = bank.number;
    if (obo) obo.textContent = bank.owner;
    document.getElementById('orderOverlay').classList.add('active');
}

// Hàm tính 50% tiền đặt cọc
function calcDeposit(priceStr) {
    // Parse số từ chuỗi giá: "69.000đ" -> 69000
    var num = priceStr.replace(/[^0-9]/g, '');
    var amount = parseInt(num) || 0;
    if (amount === 0) return 'Liên hệ Admin';
    var deposit = Math.floor(amount * 0.5);
    return new Intl.NumberFormat('vi-VN').format(deposit) + 'đ';
}

// Event listener cho checkbox (thêm vào script)
function initOrderCheckbox() {
    var cb = document.getElementById('order-confirm-deposit');
    if (!cb) return;
    cb.addEventListener('change', function() {
        var btn = document.getElementById('btn-order-submit');
        if (btn) {
            btn.disabled = !this.checked;
            btn.style.opacity = this.checked ? '1' : '0.4';
        }
    });
}

function closeOrderForm() {
    document.getElementById('orderOverlay').classList.remove('active');
}

function submitOrder() {
    var currentUser = getCurrentUser();
    var productTitle = window._orderProduct ? window._orderProduct.title : 'Gói cày thuê';
    var productPrice = window._orderProduct ? window._orderProduct.price : 'Liên hệ';
    var customerName = document.getElementById('order-customer-name').value.trim();
    var customerPhone = document.getElementById('order-customer-phone').value.trim();
    var note = document.getElementById('order-note').value.trim();
    var depositAmount = document.getElementById('orderDepositAmount').textContent;
    
    if (!customerName) {
        showToast('⚠️ Vui lòng nhập họ tên của bạn!', 'warning');
        return;
    }
    if (!customerPhone) {
        showToast('⚠️ Vui lòng nhập số điện thoại!', 'warning');
        return;
    }
    
    // ✅ Kiểm tra checkbox xác nhận đặt cọc
    var confirmDeposit = document.getElementById('order-confirm-deposit');
    if (!confirmDeposit || !confirmDeposit.checked) {
        showToast('⚠️ Vui lòng xác nhận đã chuyển khoản tiền đặt cọc!', 'warning');
        return;
    }
    
    // Đóng form
    closeOrderForm();
    
    // Gửi thông báo qua Discord với đầy đủ thông tin (có deposit)
    notifyPurchaseFull(currentUser, productTitle, productPrice, customerName, customerPhone, note, depositAmount);
    
    // ✅ LƯU ĐƠN HÀNG vào localStorage để Admin quản lý
    saveServiceOrder(currentUser, productTitle, productPrice, depositAmount, customerName, customerPhone, note);
    
    showToast('✅ Đã gửi đơn! Admin sẽ kiểm tra và liên hệ bạn qua SĐT sớm nhất.', 'success');
}

// ==========================================
// QUẢN LÝ ĐƠN ĐẶT DỊCH VỤ (CÓ CỌC)
// ==========================================

function getServiceOrders() {
    try {
        return JSON.parse(localStorage.getItem('shop_service_orders')) || [];
    } catch(e) {
        return [];
    }
}

function saveServiceOrders(orders) {
    localStorage.setItem('shop_service_orders', JSON.stringify(orders));
}

function saveServiceOrder(username, productTitle, productPrice, depositAmount, customerName, customerPhone, note) {
    var orders = getServiceOrders();
    var now = new Date().toISOString();
    var order = {
        id: 'ORD-' + Date.now() + '-' + Math.random().toString(36).substr(2,4),
        username: username,
        productTitle: productTitle,
        productPrice: productPrice,
        depositAmount: depositAmount,
        customerName: customerName,
        customerPhone: customerPhone,
        note: note,
        status: 'pending', // pending → deposited → done / cancelled
        createdAt: now,
        depositedAt: null,
        doneAt: null
    };
    orders.unshift(order);
    saveServiceOrders(orders);
}

function renderAdminOrders() {
    var container = document.getElementById('admin-order-list');
    if (!container) return;
    
    var orders = getServiceOrders();
    var activeOrders = orders.filter(function(o) { return o.status === 'pending' || o.status === 'deposited'; });
    var archiveOrders = orders.filter(function(o) { return o.status === 'done' || o.status === 'cancelled'; });
    
    var badge = document.getElementById('admin-order-badge');
    if (badge) badge.textContent = activeOrders.length;
    
    if (activeOrders.length === 0) {
        container.innerHTML = '<p style="text-align:center;color:var(--secondary-text);padding:15px">✅ Không có đơn nào đang xử lý</p>' +
            (archiveOrders.length > 0 ? '<div style="text-align:center;margin-top:8px"><button class="btn-show-archive" onclick="renderArchivedOrders()">📂 Xem lịch sử đơn (' + archiveOrders.length + ')</button></div>' : '');
    } else {
        var html = '';
        var statusLabels = { pending: '⏳ Chờ xác nhận cọc', deposited: '🔒 Đã nhận cọc' };
        
        activeOrders.forEach(function(o) {
            var timeStr = new Date(o.createdAt).toLocaleString('vi-VN');
            var depositStr = o.depositedAt ? '<br>🕐 Nhận cọc: ' + new Date(o.depositedAt).toLocaleString('vi-VN') : '';
            
            html += '<div class="order-request-item">' +
                '<div class="or-header">' +
                    '<span class="or-user">🛒 ' + escapeHTML(o.productTitle) + '</span>' +
                    '<span class="or-status ' + o.status + '">' + (statusLabels[o.status] || o.status) + '</span>' +
                '</div>' +
                '<div class="or-info">' +
                    '👤 KH: <b>' + escapeHTML(o.customerName) + '</b> | 📱 <b>' + escapeHTML(o.customerPhone) + '</b><br>' +
                    '💰 Giá gốc: ' + escapeHTML(o.productPrice) + ' | 🔒 <span class="or-deposit">Cọc: ' + escapeHTML(o.depositAmount) + '</span><br>' +
                    '📝 Ghi chú: ' + escapeHTML(o.note || '(không có)') + '<br>' +
                    '🕐 Đặt lúc: ' + timeStr + depositStr +
                '</div>' +
                '<div class="or-actions">';
            
            if (o.status === 'pending') {
                html += '<button class="btn-confirm-deposit" data-oid="' + o.id + '">💰 Xác nhận đã nhận cọc</button>' +
                        '<button class="btn-order-cancel-admin" data-oid="' + o.id + '">❌ Hủy</button>';
            } else if (o.status === 'deposited') {
                html += '<button class="btn-order-done" data-oid="' + o.id + '">✅ Hoàn thành (Thu nốt)</button>' +
                        '<button class="btn-order-cancel-admin" data-oid="' + o.id + '">❌ Hủy</button>';
            }
            
            html += '</div></div>';
        });
        
        if (archiveOrders.length > 0) {
            html += '<div style="text-align:center;margin-top:10px;padding-top:10px;border-top:1px solid #30363d"><button class="btn-show-archive" onclick="renderArchivedOrders()">📂 Xem lịch sử đơn (' + archiveOrders.length + ')</button></div>';
        }
        
        container.innerHTML = html;
        
        // Attach event listeners
        var confirmBtns = container.querySelectorAll('.btn-confirm-deposit');
        var doneBtns = container.querySelectorAll('.btn-order-done');
        var cancelBtns = container.querySelectorAll('.btn-order-cancel-admin');
        
        confirmBtns.forEach(function(btn) {
            btn.addEventListener('click', function() {
                confirmDepositOrder(this.getAttribute('data-oid'));
            });
        });
        doneBtns.forEach(function(btn) {
            btn.addEventListener('click', function() {
                completeOrder(this.getAttribute('data-oid'));
            });
        });
        cancelBtns.forEach(function(btn) {
            btn.addEventListener('click', function() {
                cancelServiceOrder(this.getAttribute('data-oid'));
            });
        });
    }
}

// Hiển thị popup lịch sử đơn đã hoàn thành / hủy (có nút xóa)
function renderArchivedOrders() {
    var orders = getServiceOrders();
    var archiveOrders = orders.filter(function(o) { return o.status === 'done' || o.status === 'cancelled'; });
    
    if (archiveOrders.length === 0) {
        showToast('📂 Không có đơn nào trong lịch sử', 'info');
        return;
    }
    
    var statusLabels = { done: '✅ Hoàn thành', cancelled: '❌ Đã hủy' };
    var html = '<div style="max-height:60vh;overflow-y:auto">';
    
    archiveOrders.forEach(function(o) {
        var timeStr = new Date(o.createdAt).toLocaleString('vi-VN');
        var depositStr = o.depositedAt ? ' | 🔒 Nhận cọc: ' + new Date(o.depositedAt).toLocaleString('vi-VN') : '';
        var doneStr = o.doneAt ? ' | 🎉 Xong: ' + new Date(o.doneAt).toLocaleString('vi-VN') : '';
        
        html += '<div class="order-request-item">' +
            '<div class="or-header">' +
                '<span class="or-user">🛒 ' + escapeHTML(o.productTitle) + '</span>' +
                '<span class="or-status ' + o.status + '">' + (statusLabels[o.status] || o.status) + '</span>' +
            '</div>' +
            '<div class="or-info">' +
                '👤 KH: <b>' + escapeHTML(o.customerName) + '</b> | 📱 <b>' + escapeHTML(o.customerPhone) + '</b><br>' +
                '💰 ' + escapeHTML(o.productPrice) + ' | 🔒 Cọc: ' + escapeHTML(o.depositAmount) +
                '<br>🕐 ' + timeStr + depositStr + doneStr +
            '</div>' +
            '<div class="or-actions">' +
                '<button class="btn-order-delete" data-oid="' + o.id + '">🗑️ Xóa vĩnh viễn</button>' +
            '</div>' +
        '</div>';
    });
    
    html += '</div>' +
        '<div style="text-align:center;margin-top:10px"><button class="btn-order-cancel-admin" onclick="closeArchivePopup()">✕ Đóng</button></div>';
    
    // Tạo popup tạm
    var overlay = document.createElement('div');
    overlay.id = 'archivePopupOverlay';
    overlay.style.cssText = 'position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);z-index:9999;display:flex;justify-content:center;align-items:center;backdrop-filter:blur(4px)';
    overlay.innerHTML = '<div style="background:var(--bg-card);border:2px solid var(--primary);border-radius:16px;padding:20px;max-width:450px;width:92%;max-height:85vh;overflow:hidden">' +
        '<h3 style="color:#fff;text-align:center;margin-bottom:15px">📂 Lịch Sử Đơn Hàng (' + archiveOrders.length + ')</h3>' +
        html +
        '</div>';
    overlay.addEventListener('click', function(e) {
        if (e.target === overlay) { overlay.remove(); }
    });
    document.body.appendChild(overlay);
    
    // Attach delete listeners
    var deleteBtns = overlay.querySelectorAll('.btn-order-delete');
    deleteBtns.forEach(function(btn) {
        btn.addEventListener('click', function() {
            deleteServiceOrder(this.getAttribute('data-oid'));
            overlay.remove();
            renderArchivedOrders();
        });
    });
}

function closeArchivePopup() {
    var ov = document.getElementById('archivePopupOverlay');
    if (ov) ov.remove();
}

// Xóa vĩnh viễn đơn hàng
function deleteServiceOrder(orderId) {
    if (!confirm('🗑️ Xóa vĩnh viễn đơn này? Không thể hoàn tác!')) return;
    var orders = getServiceOrders();
    var newOrders = orders.filter(function(o) { return o.id !== orderId; });
    saveServiceOrders(newOrders);
    showToast('🗑️ Đã xóa đơn hàng!', 'info');
    renderAdminOrders();
}

function confirmDepositOrder(orderId) {
    if (!confirm('Xác nhận đã NHẬN TIỀN CỌC cho đơn này?')) return;
    var orders = getServiceOrders();
    var found = orders.find(function(o) { return o.id === orderId; });
    if (!found || found.status !== 'pending') {
        showToast('⚠️ Đơn này đã được xử lý!', 'warning');
        renderAdminOrders();
        return;
    }
    found.status = 'deposited';
    found.depositedAt = new Date().toISOString();
    saveServiceOrders(orders);
    showToast('💰 Đã xác nhận nhận cọc ' + found.depositAmount + ' từ ' + found.customerName, 'success');
    
    // Gửi thông báo Discord: đã nhận cọc
    var embed = {
        title: '💰 ĐÃ NHẬN TIỀN CỌC!',
        color: 16776960,
        fields: [
            { name: '📦 Gói', value: found.productTitle, inline: true },
            { name: '👤 KH', value: found.customerName, inline: true },
            { name: '🔒 Tiền cọc', value: found.depositAmount, inline: true },
            { name: '💰 Còn lại', value: found.productPrice + ' (thu sau khi xong)', inline: true },
            { name: '📱 SĐT', value: found.customerPhone, inline: true }
        ],
        footer: { text: '⚠️ Tiến hành cày và thu nốt sau khi hoàn thành!' },
        timestamp: new Date().toISOString()
    };
    sendDiscordNotification(embed);
    
    renderAdminOrders();
}

function completeOrder(orderId) {
    if (!confirm('Đơn này đã HOÀN THÀNH? Xác nhận để đánh dấu done và thu nốt tiền.')) return;
    var orders = getServiceOrders();
    var found = orders.find(function(o) { return o.id === orderId; });
    if (!found || found.status !== 'deposited') {
        showToast('⚠️ Đơn này chưa được xác nhận cọc!', 'warning');
        renderAdminOrders();
        return;
    }
    found.status = 'done';
    found.doneAt = new Date().toISOString();
    saveServiceOrders(orders);
    showToast('🎉 Đơn hoàn thành! Liên hệ KH thu nốt tiền còn lại.', 'success');
    
    // Gửi Discord: hoàn thành
    var embed = {
        title: '🎉 ĐƠN HOÀN THÀNH!',
        color: 3066993,
        fields: [
            { name: '📦 Gói', value: found.productTitle, inline: true },
            { name: '👤 KH', value: found.customerName, inline: true },
            { name: '📱 SĐT', value: found.customerPhone, inline: true },
            { name: '🔒 Đã thu cọc', value: found.depositAmount, inline: true },
            { name: '💰 Cần thu thêm', value: found.productPrice, inline: true },
            { name: '✅ Trạng thái', value: 'Hoàn thành - Chờ thu nốt', inline: false }
        ],
        footer: { text: '💸 Liên hệ KH thu nốt tiền rồi bàn giao!' },
        timestamp: new Date().toISOString()
    };
    sendDiscordNotification(embed);
    
    renderAdminOrders();
}

function cancelServiceOrder(orderId) {
    if (!confirm('Hủy đơn này? Thao tác này không thể hoàn tác.')) return;
    var orders = getServiceOrders();
    var found = orders.find(function(o) { return o.id === orderId; });
    if (!found) return;
    found.status = 'cancelled';
    saveServiceOrders(orders);
    showToast('❌ Đã hủy đơn hàng.', 'warning');
    renderAdminOrders();
}

// Hàm gửi Discord đầy đủ thông tin khách
function notifyPurchaseFull(username, productTitle, productPrice, customerName, customerPhone, note, depositAmount) {
    var now = new Date().toLocaleString('vi-VN', { timeZone: 'Asia/Ho_Chi_Minh' });
    var embed = {
        title: '🛒 ĐƠN ĐẶT DỊCH VỤ MỚI!',
        color: 5814783,
        fields: [
            { name: '👤 Tài khoản', value: username, inline: true },
            { name: '📦 Gói dịch vụ', value: productTitle, inline: true },
            { name: '💰 Giá gốc', value: productPrice, inline: true },
            { name: '🔒 ĐẶT CỌC 50%', value: depositAmount || 'N/A', inline: true },
            { name: '🧑 Họ tên KH', value: customerName, inline: true },
            { name: '📱 Số điện thoại', value: customerPhone, inline: true },
            { name: '✅ Trạng thái', value: 'Đã xác nhận chuyển cọc', inline: false },
            { name: '📝 Ghi chú', value: note || '(không có)', inline: false },
            { name: '🕐 Thời gian', value: now, inline: false }
        ],
        footer: { text: '⚠️ Admin kiểm tra tài khoản và liên hệ KH ngay!' },
        timestamp: new Date().toISOString()
    };
    sendDiscordNotification(embed);
}
// ==========================================
// DISCORD NOTIFICATION
// ==========================================
function sendDiscordNotification(embed) {
    var payload = JSON.stringify({
        embeds: [embed]
    });
    
    fetch(DISCORD_WEBHOOK, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: payload
    }).then(function(res) {
        if (res.ok) console.log('Discord notification sent!');
        else console.warn('Discord notification failed:', res.status);
    }).catch(function(err) {
        console.warn('Discord notification error:', err);
    });
}

function notifyCardSubmit(username, telco, denom, realAmount, cardCode, cardSerial) {
    var now = new Date().toLocaleString('vi-VN', { timeZone: 'Asia/Ho_Chi_Minh' });
    var embed = {
        title: '📱 NẠP THẺ CÀO MỚI!',
        color: 14689513,
        fields: [
            { name: '👤 Khách hàng', value: username, inline: true },
            { name: '📶 Nhà mạng', value: telco.toUpperCase(), inline: true },
            { name: '💵 Mệnh giá', value: new Intl.NumberFormat('vi-VN').format(denom) + 'đ', inline: true },
            { name: '🎁 Thực nhận', value: new Intl.NumberFormat('vi-VN').format(realAmount) + 'đ', inline: true },
            { name: '🔢 Mã thẻ', value: '||' + cardCode + '||', inline: true },
            { name: '📋 Seri', value: '||' + cardSerial + '||', inline: true },
            { name: '🕐 Thời gian', value: now, inline: false }
        ],
        footer: { text: 'Blox Shop - Chờ Admin duyệt' },
        timestamp: new Date().toISOString()
    };
    sendDiscordNotification(embed);
}

function notifyAdminApproved(adminName, customerName, amount) {
    var now = new Date().toLocaleString('vi-VN', { timeZone: 'Asia/Ho_Chi_Minh' });
    var embed = {
        title: '✅ ĐÃ DUYỆT THẺ CÀO',
        color: 3066993,
        fields: [
            { name: '👑 Admin duyệt', value: adminName, inline: true },
            { name: '👤 Khách hàng', value: customerName, inline: true },
            { name: '💰 Cộng ví', value: '+' + new Intl.NumberFormat('vi-VN').format(amount) + 'đ', inline: true },
            { name: '🕐 Thời gian', value: now, inline: false }
        ],
        footer: { text: 'Blox Shop - Đã cộng tiền vào ví' },
        timestamp: new Date().toISOString()
    };
    sendDiscordNotification(embed);
}


// ==========================================
// XỬ LÝ QUAY RANDOM ACC (ĐÃ SỬA LỖI)
// ==========================================
function handleSpinRandom() {
    var currentUser = getCurrentUser();
    if (!currentUser) {
        showToast('⚠️ Vui lòng đăng nhập tài khoản trước khi Thử vận may!', 'warning');
        toggleModal('loginModal');
        return;
    }

    var config = getRandomConfig();
    var price = parseInt(config.price) || 0;

    // ✅ KIỂM TRA SỐ DƯ VÍ
    var wallet = readWallet(currentUser);
    if (wallet < price) {
        showToast('❌ Bạn không đủ Blox Xu! Cần ' + new Intl.NumberFormat('vi-VN').format(price) + 'đ, hiện có ' + new Intl.NumberFormat('vi-VN').format(wallet) + 'đ. Vui lòng nạp thêm!', 'error');
        return;
    }

    // ✅ RATE LIMIT - Chống spam random (1 lần/3 giây)
    if (!_RL.check('random_' + currentUser, 1, 3)) {
        showToast('⏳ Vui lòng đợi 3 giây trước khi quay tiếp!', 'warning');
        return;
    }

    // Validate tỉ lệ
    var totalRate = (config.rateVip || 0) + (config.rateMedium || 0) + (config.rateBasic || 0);
    if (Math.abs(totalRate - 100) > 0.01) {
        showToast('⚠️ Tỉ lệ trúng chưa chính xác (tổng=' + totalRate.toFixed(1) + '%). Vui lòng liên hệ Admin!', 'error');
        return;
    }

    // Gom tất cả các kho
    var stockMap = {
        vip: { arr: (config.stockVip || []).slice(), title: '🔥 ACC VIP V4 + MOCHI/DRAGON!' },
        medium: { arr: (config.stockMedium || []).slice(), title: '⚡ ACC GIÀU MAX LEVEL + GODHUMAN!' },
        basic: { arr: (config.stockBasic || []).slice(), title: '👑 Acc Thường Ngẫu Nhiên' }
    };

    // Kiểm tra có ít nhất 1 acc không
    var totalAcc = stockMap.vip.arr.length + stockMap.medium.arr.length + stockMap.basic.arr.length;
    if (totalAcc === 0) {
        showToast('❌ Kho Acc đã hết! Vui lòng liên hệ Admin để nạp thêm!', 'error');
        return;
    }

    // Roll random
    var roll = Math.floor(Math.random() * 100) + 1;
    var selectedTier;

    if (roll <= config.rateVip && stockMap.vip.arr.length > 0) {
        selectedTier = 'vip';
    } else if (roll <= (config.rateVip + config.rateMedium) && stockMap.medium.arr.length > 0) {
        selectedTier = 'medium';
    } else if (stockMap.basic.arr.length > 0) {
        selectedTier = 'basic';
    } else if (stockMap.medium.arr.length > 0) {
        selectedTier = 'medium';
    } else if (stockMap.vip.arr.length > 0) {
        selectedTier = 'vip';
    } else {
        showToast('❌ Kho Acc đã hết! Vui lòng liên hệ Admin!', 'error');
        return;
    }

    var tierData = stockMap[selectedTier];
    var accLine = tierData.arr.shift();

    // Ghi lại
    config.stockVip = stockMap.vip.arr;
    config.stockMedium = stockMap.medium.arr;
    config.stockBasic = stockMap.basic.arr;
    config.sold = (config.sold || 0) + 1;
    saveRandomConfig(config);

    // ✅ TRỪ TIỀN TRONG VÍ
    var newBalance = wallet - price;
    writeWallet(currentUser, newBalance);
    updateWalletDisplay();

    renderShopProducts();

    // Parse
    var parts = accLine.split('|');
    var username = (parts[0] || '').trim() || 'không_rõ';
    var password = (parts[1] || '').trim() || 'không_rõ';

    var rankEl = document.getElementById('res-acc-rank');
    var userEl = document.getElementById('res-acc-user');
    var passEl = document.getElementById('res-acc-pass');
    if (rankEl) rankEl.innerText = tierData.title;
    if (userEl) userEl.innerText = username;
    if (passEl) passEl.innerText = password;

    toggleModal('randomResultModal');
    showToast('🎉 Quay thành công! -' + new Intl.NumberFormat('vi-VN').format(price) + 'đ', 'success');
}

function copyAccInfo() {
    var userEl = document.getElementById('res-acc-user');
    var passEl = document.getElementById('res-acc-pass');
    var u = userEl ? userEl.innerText : '';
    var p = passEl ? passEl.innerText : '';
    var text = 'Tài khoản: ' + u + ' | Mật khẩu: ' + p;

    navigator.clipboard.writeText(text).then(function() {
        showToast('📋 Đã sao chép Tài khoản & Mật khẩu!', 'success');
    }).catch(function() {
        showToast('Tài khoản: ' + u + '\nMật khẩu: ' + p, 'warning');
    });
}

// ==========================================
// ADMIN PANEL
// ==========================================
function setInputValue(id, value) {
    var el = document.getElementById(id);
    if (el) el.value = (value !== undefined && value !== null) ? value : '';
}

function setTextareaValue(id, value) {
    var el = document.getElementById(id);
    if (el) el.value = (value !== undefined && value !== null) ? value : '';
}

function getInputValue(id, defaultVal) {
    var el = document.getElementById(id);
    return (el && el.value.trim()) ? el.value.trim() : (defaultVal || '');
}

function getTextareaLines(id) {
    var el = document.getElementById(id);
    if (!el || !el.value.trim()) return [];
    return el.value.split('\n').map(function(s) { return s.trim(); }).filter(function(s) { return s.length > 0; });
}

function openAdminModal() {
    var config = getRandomConfig();
    setInputValue('admin-random-title-input', config.title);
    setInputValue('admin-random-price-input', config.price);
    setInputValue('admin-random-sold-input', config.sold);
    setInputValue('admin-rate-vip', config.rateVip);
    setInputValue('admin-rate-medium', config.rateMedium);
    setInputValue('admin-rate-basic', config.rateBasic);
    setTextareaValue('admin-stock-vip', (config.stockVip || []).join('\n'));
    setTextareaValue('admin-stock-medium', (config.stockMedium || []).join('\n'));
    setTextareaValue('admin-stock-basic', (config.stockBasic || []).join('\n'));

    setInputValue('admin-notice-input', localStorage.getItem(LS_KEYS.NOTICE) || 'Chào mừng bạn đến với Shop Blox Fruit chính thức!');
    
    // Load announcement settings
    document.getElementById('admin-ann-enabled').checked = localStorage.getItem(LS_KEYS.ANNOUNCEMENT_ENABLED) === 'true';
    document.getElementById('admin-ann-type').value = localStorage.getItem(LS_KEYS.ANNOUNCEMENT_TYPE) || 'info';
    setInputValue('admin-ann-text', localStorage.getItem(LS_KEYS.ANNOUNCEMENT_TEXT) || '');
    document.getElementById('admin-popup-enabled').checked = localStorage.getItem(LS_KEYS.POPUP_ENABLED) === 'true';
    setInputValue('admin-popup-icon', localStorage.getItem(LS_KEYS.POPUP_ICON) || '📢');
    setInputValue('admin-popup-title', localStorage.getItem(LS_KEYS.POPUP_TITLE) || '');
    setInputValue('admin-popup-body', localStorage.getItem(LS_KEYS.POPUP_BODY) || '');
    setInputValue('admin-popup-title-size', localStorage.getItem(LS_KEYS.POPUP_TITLE_SIZE) || '24');
    setInputValue('admin-popup-title-color', localStorage.getItem(LS_KEYS.POPUP_TITLE_COLOR) || '#ffffff');
    setInputValue('admin-popup-body-size', localStorage.getItem(LS_KEYS.POPUP_BODY_SIZE) || '15');
    setInputValue('admin-popup-body-color', localStorage.getItem(LS_KEYS.POPUP_BODY_COLOR) || '#c9d1d9');
    toggleAnnSettings();
    togglePopupSettings();
    setInputValue('admin-banner-title-input', localStorage.getItem(LS_KEYS.BANNER_TITLE) || '🔥 SIÊU SALE 70%');
    setInputValue('admin-banner-code-input', localStorage.getItem(LS_KEYS.BANNER_CODE) || 'BAOZI70');

    var bank = getBankInfo();
    setInputValue('admin-bank-name', bank.name);
    setInputValue('admin-bank-number', bank.number);
    setInputValue('admin-bank-owner', bank.owner);
    setInputValue('admin-bank-note', bank.note);
    setInputValue('admin-contact-phone', localStorage.getItem(LS_KEYS.SHOP_PHONE) || SHOP_PHONE);
    setInputValue('admin-contact-facebook', localStorage.getItem(LS_KEYS.SHOP_FACEBOOK) || SHOP_FACEBOOK);

    // Load maintenance settings
    document.getElementById('admin-maintenance-enabled').checked = localStorage.getItem(LS_KEYS.MAINTENANCE_ENABLED) === 'true';
    document.getElementById('admin-maintenance-until').value = localStorage.getItem(LS_KEYS.MAINTENANCE_UNTIL) || '';
    setInputValue('admin-maintenance-title', localStorage.getItem(LS_KEYS.MAINTENANCE_TITLE) || 'Hệ thống đang bảo trì');
    setInputValue('admin-maintenance-body', localStorage.getItem(LS_KEYS.MAINTENANCE_BODY) || '');
    toggleMaintenanceSettings();
    
    validateRateSum();
    updateStockCounts();
    updateStockDisplay();
    renderAdminProductsList();
    renderAdminCardRequests();
    renderAdminOrders();
    populateAdminPasswordSelect();
    updateCardBadge();

    var users = getUsers();
    var totalUsersEl = document.getElementById('admin-total-users');
    if (totalUsersEl) totalUsersEl.innerText = Object.keys(users).length;

    toggleModal('adminModal');
}

function renderAdminProductsList() {
    var products = getProducts();
    var container = document.getElementById('admin-products-list');
    if (!container) return;
    container.innerHTML = '';

    products.forEach(function(p) {
        var itemHTML = '<div class="admin-prod-card" data-id="' + p.id + '">' +
            '<div class="form-group"><label>Tên gói</label><input type="text" class="edit-title" value="' + escapeHTML(p.title) + '"></div>' +
            '<div class="admin-grid-2"><div class="form-group"><label>Giá bán</label><input type="text" class="edit-price" value="' + escapeHTML(p.price) + '"></div>' +
            '<div class="form-group"><label>Giá cũ</label><input type="text" class="edit-oldprice" value="' + escapeHTML(p.priceOld || '') + '"></div></div>' +
            '<div class="admin-grid-2"><div class="form-group"><label>Giảm giá</label><input type="text" class="edit-discount" value="' + escapeHTML(p.discount || '') + '"></div>' +
            '<div class="form-group"><label>Đã bán</label><input type="number" class="edit-sold" value="' + (p.sold || 0) + '" min="0"></div></div>' +
            '<button type="button" class="btn-danger" style="padding:6px;font-size:12px" onclick="handleDeleteProduct(' + p.id + ')">🗑️ Xóa gói này</button>' +
            '</div>';
        container.insertAdjacentHTML('beforeend', itemHTML);
    });
}

function handleAddNewProduct() {
    var name = document.getElementById('new-prod-name').value.trim();
    var price = document.getElementById('new-prod-price').value.trim();
    var oldPrice = document.getElementById('new-prod-oldprice').value.trim();
    var discount = document.getElementById('new-prod-discount').value.trim();
    var sold = parseInt(document.getElementById('new-prod-sold').value) || 0;

    if (!name || !price) {
        showToast('⚠️ Vui lòng nhập Tên dịch vụ và Giá bán!', 'warning');
        return;
    }

    var products = getProducts();
    var newId = products.length > 0 ? Math.max.apply(null, products.map(function(p) { return p.id; })) + 1 : 1;

    products.push({ id: newId, title: name, price: price, priceOld: oldPrice, discount: discount, sold: sold });
    saveProducts(products);
    renderAdminProductsList();
    renderShopProducts();

    document.getElementById('new-prod-name').value = '';
    document.getElementById('new-prod-price').value = '';
    document.getElementById('new-prod-oldprice').value = '';
    document.getElementById('new-prod-discount').value = '';
    document.getElementById('new-prod-sold').value = '';
    showToast('✨ Đã thêm gói cày thuê mới!', 'success');
}

function handleDeleteProduct(id) {
    if (!confirm('Bạn có chắc muốn xóa gói dịch vụ này?')) return;
    var products = getProducts().filter(function(p) { return p.id !== id; });
    saveProducts(products);
    renderAdminProductsList();
    renderShopProducts();
    showToast('🗑️ Đã xóa gói dịch vụ!', 'success');
}

// ==========================================
// SAVE ADMIN CHANGES (ĐÃ SỬA LỖI BANNER)
// ==========================================

function toggleAnnSettings() {
    var checked = document.getElementById('admin-ann-enabled').checked;
    document.getElementById('admin-ann-type').disabled = !checked;
    document.getElementById('admin-ann-text').disabled = !checked;
}

function togglePopupSettings() {
    var checked = document.getElementById('admin-popup-enabled').checked;
    var fields = ['admin-popup-icon', 'admin-popup-title', 'admin-popup-body', 'admin-popup-title-size', 'admin-popup-title-color', 'admin-popup-body-size', 'admin-popup-body-color'];
    for (var i = 0; i < fields.length; i++) {
        var el = document.getElementById(fields[i]);
        if (el) el.disabled = !checked;
    }
}

function toggleMaintenanceSettings() {
    var checked = document.getElementById('admin-maintenance-enabled').checked;
    var container = document.getElementById('maintenance-settings');
    if (container) container.style.display = checked ? 'block' : 'none';
}

function saveAdminChanges() {
    // Validate tỉ lệ
    var rateVip = parseFloat(document.getElementById('admin-rate-vip').value) || 0;
    var rateMedium = parseFloat(document.getElementById('admin-rate-medium').value) || 0;
    var rateBasic = parseFloat(document.getElementById('admin-rate-basic').value) || 0;
    var totalRate = rateVip + rateMedium + rateBasic;
    if (Math.abs(totalRate - 100) > 0.01) {
        showToast('⚠️ Tỉ lệ trúng phải có tổng = 100%! Hiện tại: ' + totalRate.toFixed(1) + '%', 'error');
        return;
    }

    // Save random config
    var config = getRandomConfig();
    config.title = getInputValue('admin-random-title-input', config.title);
    config.price = getInputValue('admin-random-price-input', config.price);
    config.sold = parseInt(document.getElementById('admin-random-sold-input').value) || config.sold;
    config.rateVip = rateVip;
    config.rateMedium = rateMedium;
    config.rateBasic = rateBasic;
    config.stockVip = getTextareaLines('admin-stock-vip');
    config.stockMedium = getTextareaLines('admin-stock-medium');
    config.stockBasic = getTextareaLines('admin-stock-basic');
    saveRandomConfig(config);

    // Save notice & banner — ĐÃ SỬA: đọc từ INPUT admin, không từ innerText
    var annEnabled = document.getElementById('admin-ann-enabled').checked;
    localStorage.setItem(LS_KEYS.ANNOUNCEMENT_ENABLED, annEnabled);
    localStorage.setItem(LS_KEYS.ANNOUNCEMENT_TYPE, document.getElementById('admin-ann-type').value);
    localStorage.setItem(LS_KEYS.ANNOUNCEMENT_TEXT, getInputValue('admin-ann-text', ''));
    
    var popupEnabled = document.getElementById('admin-popup-enabled').checked;
    localStorage.setItem(LS_KEYS.POPUP_ENABLED, popupEnabled);
    localStorage.setItem(LS_KEYS.POPUP_ICON, getInputValue('admin-popup-icon', '📢'));
    localStorage.setItem(LS_KEYS.POPUP_TITLE, getInputValue('admin-popup-title', ''));
    localStorage.setItem(LS_KEYS.POPUP_BODY, getInputValue('admin-popup-body', ''));
    localStorage.setItem(LS_KEYS.POPUP_TITLE_SIZE, getInputValue('admin-popup-title-size', '24'));
    localStorage.setItem(LS_KEYS.POPUP_TITLE_COLOR, getInputValue('admin-popup-title-color', '#ffffff'));
    localStorage.setItem(LS_KEYS.POPUP_BODY_SIZE, getInputValue('admin-popup-body-size', '15'));
    localStorage.setItem(LS_KEYS.POPUP_BODY_COLOR, getInputValue('admin-popup-body-color', '#c9d1d9'));
    
    var noticeVal = getInputValue('admin-notice-input', '');
    var bannerTitleVal = getInputValue('admin-banner-title-input', '');
    var bannerCodeVal = getInputValue('admin-banner-code-input', '');
    if (noticeVal) localStorage.setItem(LS_KEYS.NOTICE, noticeVal);
    if (bannerTitleVal) localStorage.setItem(LS_KEYS.BANNER_TITLE, bannerTitleVal);
    if (bannerCodeVal) localStorage.setItem(LS_KEYS.BANNER_CODE, bannerCodeVal);

    // Save bank info
    var bank = {
        name: getInputValue('admin-bank-name', DEFAULT_BANK.name),
        number: getInputValue('admin-bank-number', DEFAULT_BANK.number),
        owner: getInputValue('admin-bank-owner', DEFAULT_BANK.owner),
        note: getInputValue('admin-bank-note', DEFAULT_BANK.note)
    };
    saveBankInfo(bank);
    localStorage.setItem(LS_KEYS.SHOP_PHONE, getInputValue('admin-contact-phone', SHOP_PHONE));
    localStorage.setItem(LS_KEYS.SHOP_FACEBOOK, getInputValue('admin-contact-facebook', SHOP_FACEBOOK));

    // Save product edits
    var cards = document.querySelectorAll('.admin-prod-card');
    var products = getProducts();
    cards.forEach(function(card) {
        var id = parseInt(card.getAttribute('data-id'));
        var titleInput = card.querySelector('.edit-title');
        var priceInput = card.querySelector('.edit-price');
        var oldPriceInput = card.querySelector('.edit-oldprice');
        var discountInput = card.querySelector('.edit-discount');
        var soldInput = card.querySelector('.edit-sold');

        var p = products.find(function(item) { return item.id === id; });
        if (p) {
            if (titleInput) p.title = titleInput.value.trim();
            if (priceInput) p.price = priceInput.value.trim();
            if (oldPriceInput) p.priceOld = oldPriceInput.value.trim();
            if (discountInput) p.discount = discountInput.value.trim();
            if (soldInput) p.sold = parseInt(soldInput.value) || 0;
        }
    });
    saveProducts(products);

    var phoneVal = getInputValue('admin-contact-phone', SHOP_PHONE);
    var fbVal = getInputValue('admin-contact-facebook', SHOP_FACEBOOK);
    if (phoneVal) { localStorage.setItem(LS_KEYS.SHOP_PHONE, phoneVal); localStorage.setItem('shop_phone', phoneVal); }
    if (fbVal) { localStorage.setItem(LS_KEYS.SHOP_FACEBOOK, fbVal); localStorage.setItem('shop_facebook', fbVal); }

    // Save maintenance
    var maintenanceEnabled = document.getElementById('admin-maintenance-enabled').checked;
    localStorage.setItem(LS_KEYS.MAINTENANCE_ENABLED, maintenanceEnabled);
    localStorage.setItem(LS_KEYS.MAINTENANCE_UNTIL, document.getElementById('admin-maintenance-until').value);
    localStorage.setItem(LS_KEYS.MAINTENANCE_TITLE, getInputValue('admin-maintenance-title', 'Hệ thống đang bảo trì'));
    localStorage.setItem(LS_KEYS.MAINTENANCE_BODY, getInputValue('admin-maintenance-body', ''));

    renderShopProducts();
    toggleModal('adminModal');
    showToast('💾 Đã lưu tất cả thay đổi thành công!', 'success');
}

// ==========================================
// AUTH
// ==========================================
function handleLogin() {
    // ✅ RATE LIMIT - Chống brute force (3 lần/30 giây)
    if (!_RL.check('login_attempt', 3, 30)) {
        showToast('⏳ Quá nhiều lần thử! Vui lòng đợi 30 giây.', 'warning');
        return;
    }

    var u = document.getElementById('login-username').value.trim();
    var p = document.getElementById('login-password').value.trim();

    if (!u || !p) {
        showToast('⚠️ Vui lòng nhập đầy đủ tên đăng nhập và mật khẩu!', 'warning');
        return;
    }

    if (isAdmin(u, p)) {
        setCurrentUser(u);
        toggleModal('loginModal');
        updateUIAuthState();
        _resetSessionTimer();
        showToast('👑 Chào mừng Quản Trị Viên!', 'success');
        openAdminModal();
        return;
    }

    var users = getUsers();
    if (users[u] && users[u].password === p) {
        setCurrentUser(u);
        toggleModal('loginModal');
        updateUIAuthState();
        _resetSessionTimer();
        showToast('🎉 Đăng nhập thành công!', 'success');
        renderCardForm();
    } else {
        showToast('❌ Tên đăng nhập hoặc mật khẩu không chính xác!', 'error');
    }
}

function handleRegister() {
    var u = document.getElementById('reg-username').value.trim();
    var p = document.getElementById('reg-password').value.trim();
    var rp = document.getElementById('reg-repassword').value.trim();

    if (!u || !p || !rp) {
        showToast('⚠️ Vui lòng điền đầy đủ thông tin đăng ký!', 'warning');
        return;
    }
    if (u.length < 3) {
        showToast('⚠️ Tên đăng nhập phải có ít nhất 3 ký tự!', 'warning');
        return;
    }
    if (p.length < 6) {
        showToast('⚠️ Mật khẩu phải có ít nhất 6 ký tự!', 'warning');
        return;
    }
    if (p !== rp) {
        showToast('❌ Mật khẩu xác nhận không khớp!', 'error');
        return;
    }
    if (isAdminUser(u)) {
        showToast('❌ Tên đăng nhập này thuộc về hệ thống!', 'error');
        return;
    }

    // ✅ RATE LIMIT - Chống spam đăng ký (1 lần/30 giây)
    if (!_RL.check('register', 1, 30)) {
        showToast('⏳ Vui lòng đợi 30 giây trước khi đăng ký tiếp!', 'warning');
        return;
    }

    var users = getUsers();
    if (users[u]) {
        showToast('❌ Tên đăng nhập đã tồn tại!', 'error');
        return;
    }

    users[u] = { password: p, avatar: '' };
    saveUsers(users);
    showToast('✅ Đăng ký tài khoản thành công! Hãy đăng nhập.', 'success');
    switchModal('registerModal', 'loginModal');
}

function handleLogout() {
    setCurrentUser('');
    ['profileModal', 'adminModal'].forEach(function(id) {
        var el = document.getElementById(id);
        if (el) el.classList.remove('active');
    });
    updateUIAuthState();
    showToast('🚪 Đã đăng xuất!', 'success');
    renderCardForm();
}

function updateUIAuthState() {
    var currentUser = getCurrentUser();
    var loginBtn = document.getElementById('top-login-btn');
    var navAccountText = document.getElementById('nav-account-text');
    var navAccountIcon = document.getElementById('nav-account-icon');
    var navAccountBtn = document.getElementById('nav-account-btn');

    if (!currentUser) {
        if (loginBtn) { loginBtn.innerText = '🔑 Đăng nhập'; loginBtn.className = 'btn-login-top'; loginBtn.onclick = function() { toggleModal('loginModal'); }; }
        if (navAccountText) navAccountText.innerText = 'Tài khoản';
        if (navAccountIcon) navAccountIcon.innerHTML = '👤';
        if (navAccountBtn) navAccountBtn.onclick = function() { toggleModal('loginModal'); };
        var histBtn = document.getElementById('nav-card-history-btn');
        if (histBtn) histBtn.onclick = function() { toggleModal('loginModal'); };
    } else if (isAdminUser(currentUser)) {
        if (loginBtn) { loginBtn.innerText = '👑 Admin Panel'; loginBtn.className = 'btn-login-top is-admin'; loginBtn.onclick = openAdminModal; }
        if (navAccountText) navAccountText.innerText = 'Admin';
        if (navAccountIcon) navAccountIcon.innerHTML = '👑';
        if (navAccountBtn) navAccountBtn.onclick = openAdminModal;
        updateCardBadge();
        var histBtn = document.getElementById('nav-card-history-btn');
        if (histBtn) histBtn.onclick = openAdminModal;
    } else {
        var users = getUsers();
        var userObj = users[currentUser] || {};

        if (loginBtn) { loginBtn.innerText = '👤 ' + currentUser; loginBtn.className = 'btn-login-top'; loginBtn.onclick = openProfileModal; }
        if (navAccountText) navAccountText.innerText = currentUser.length > 8 ? currentUser.substring(0, 7) + '...' : currentUser;
        if (userObj.avatar) {
            if (navAccountIcon) navAccountIcon.innerHTML = '<img src="' + userObj.avatar + '" class="avatar-img-nav">';
        } else {
            if (navAccountIcon) navAccountIcon.innerHTML = '👤';
        }
        if (navAccountBtn) navAccountBtn.onclick = openProfileModal;
        var histBtn = document.getElementById('nav-card-history-btn');
        if (histBtn) histBtn.onclick = function() { renderCardHistory(); toggleModal('cardHistoryModal'); };
        renderCardForm();
    }
}

function openProfileModal() {
    var currentUser = getCurrentUser();
    if (!currentUser) {
        toggleModal('loginModal');
        return;
    }
    var users = getUsers();
    var userObj = users[currentUser] || {};

    var usernameEl = document.getElementById('profile-username-display');
    var preview = document.getElementById('profile-avatar-preview');
    if (usernameEl) usernameEl.innerText = currentUser;
    if (preview) preview.src = userObj.avatar || 'data:image/svg+xml,%3Csvg xmlns=%22http://www.w3.org/2000/svg%22 width=%2285%22 height=%2285%22%3E%3Crect fill=%22%2321262d%22 width=%2285%22 height=%2285%22/%3E%3Ctext fill=%22%2358a6ff%22 x=%2250%25%22 y=%2255%25%22 dominant-baseline=%22middle%22 text-anchor=%22middle%22 font-size=%2230%22%3E👤%3C/text%3E%3C/svg%3E';
    tempAvatarBase64 = userObj.avatar || null;

    toggleModal('profileModal');
}

function handleSelectAvatar(event) {
    var file = event.target.files[0];
    if (!file) return;

    var reader = new FileReader();
    reader.onload = function(e) {
        tempAvatarBase64 = e.target.result;
        var preview = document.getElementById('profile-avatar-preview');
        if (preview) preview.src = tempAvatarBase64;
    };
    reader.readAsDataURL(file);
}

function saveAvatar() {
    var currentUser = getCurrentUser();
    if (!currentUser) return;

    var users = getUsers();
    if (users[currentUser]) {
        users[currentUser].avatar = tempAvatarBase64 || '';
        saveUsers(users);
        updateUIAuthState();
        toggleModal('profileModal');
        showToast('💾 Đã lưu ảnh đại diện thành công!', 'success');
    }
}

// ==========================================
// INIT

// ==========================================
// CARD PAYMENT SYSTEM
// ==========================================

const CARD_CONFIG = {
    DISCOUNT_RATE: 0.85, // Khách nhận 85% mệnh giá
    TELCOS: ['viettel', 'mobifone', 'vinaphone'],
    DENOMS: [10000, 20000, 50000, 100000, 200000, 500000]
};

let selectedTelco = 'viettel';
let selectedDenom = 20000;

// Getter/Setter cho card requests
function getCardRequests() {
    return safeGetJSON('shop_card_requests', []);
}

function saveCardRequests(requests) {
    safeSetJSON('shop_card_requests', requests);
}

function getCardHistory(username) {
    var all = getCardRequests();
    return all.filter(function(r) { return r.username === username; });
}

function getPendingRequests() {
    var all = getCardRequests();
    return all.filter(function(r) { return r.status === 'pending'; });
}

// Wallet functions
// ⚠️ BẢO VỆ SỐ DƯ - Checksum chống chỉnh sửa localStorage
function _walletChecksum(username, amount){
    var s = username + '_wallet_' + amount + '_salt_BloxShop2026';
    var h = 0;
    for(var i=0;i<s.length;i++){ h = ((h<<5)-h)+s.charCodeAt(i); h|=0; }
    return h;
}

function readWallet(username){
    var users = getUsers();
    if(users[username]){
        var wallet = users[username].wallet || 0;
        var storedChecksum = users[username]._wchk || 0;
        var expectedChecksum = _walletChecksum(username, wallet);
        // Nếu checksum không khớp → ví bị chỉnh sửa → reset về 0
        if(storedChecksum !== expectedChecksum){
            console.warn('⚠️ Phát hiện chỉnh sửa trái phép ví của ' + username);
            users[username].wallet = 0;
            users[username]._wchk = _walletChecksum(username, 0);
            saveUsers(users);
            return 0;
        }
        return wallet;
    }
    return 0;
}

function writeWallet(username, amount){
    var users = getUsers();
    if(users[username]){
        var safeAmount = Math.max(0, Math.floor(amount)); // Chỉ lưu số nguyên
        users[username].wallet = safeAmount;
        users[username]._wchk = _walletChecksum(username, safeAmount);
        saveUsers(users);
    }
}

function getUserWallet(username) {
    return readWallet(username);
}

function setUserWallet(username, amount) {
    writeWallet(username, amount);
}

function addUserWallet(username, amount) {
    var current = getUserWallet(username);
    setUserWallet(username, current + amount);
}

// ==========================================
// ĐỔI MẬT KHẨU - USER (qua Profile)
// ==========================================
function changePasswordUser() {
    var currentUser = getCurrentUser();
    if (!currentUser) {
        showToast('⚠️ Vui lòng đăng nhập!', 'warning');
        return;
    }
    if (isAdminUser(currentUser)) {
        showToast('⚠️ Admin vui lòng đổi mật khẩu trong Admin Panel!', 'warning');
        return;
    }
    
    var oldPass = document.getElementById('prof-old-pass').value;
    var newPass = document.getElementById('prof-new-pass').value;
    var confirmPass = document.getElementById('prof-confirm-pass').value;
    
    if (!oldPass || !newPass || !confirmPass) {
        showToast('⚠️ Vui lòng điền đầy đủ các trường!', 'warning');
        return;
    }
    if (newPass.length < 6) {
        showToast('⚠️ Mật khẩu mới phải có ít nhất 6 ký tự!', 'warning');
        return;
    }
    if (newPass !== confirmPass) {
        showToast('❌ Mật khẩu xác nhận không khớp!', 'error');
        return;
    }
    
    var users = getUsers();
    var userObj = users[currentUser];
    if (!userObj || userObj.password !== oldPass) {
        showToast('❌ Mật khẩu hiện tại không đúng!', 'error');
        return;
    }
    
    userObj.password = newPass;
    saveUsers(users);
    
    // Clear fields
    document.getElementById('prof-old-pass').value = '';
    document.getElementById('prof-new-pass').value = '';
    document.getElementById('prof-confirm-pass').value = '';
    
    showToast('🔒 Đổi mật khẩu thành công!', 'success');
}

// ==========================================
// ĐỔI MẬT KHẨU - ADMIN (qua Admin Panel)
// ==========================================
function populateAdminPasswordSelect() {
    var sel = document.getElementById('admin-pw-select');
    if (!sel) return;
    
    var options = '';
    for (var i = 0; i < ADMIN_LIST.length; i++) {
        options += '<option value="' + i + '">' + escapeHTML(ADMIN_LIST[i].user) + '</option>';
    }
    sel.innerHTML = '<option value="">-- Chọn tài khoản --</option>' + options;
}

function changePasswordAdmin() {
    var sel = document.getElementById('admin-pw-select');
    var newPass = document.getElementById('admin-new-password').value;
    var confirmPass = document.getElementById('admin-confirm-password').value;
    
    if (!sel.value) {
        showToast('⚠️ Vui lòng chọn tài khoản admin cần đổi!', 'warning');
        return;
    }
    if (!newPass) {
        showToast('⚠️ Vui lòng nhập mật khẩu mới!', 'warning');
        return;
    }
    if (newPass.length < 6) {
        showToast('⚠️ Mật khẩu mới phải có ít nhất 6 ký tự!', 'warning');
        return;
    }
    if (newPass !== confirmPass) {
        showToast('❌ Mật khẩu xác nhận không khớp!', 'error');
        return;
    }
    
    var idx = parseInt(sel.value);
    if (isNaN(idx) || idx < 0 || idx >= ADMIN_LIST.length) {
        showToast('⚠️ Tài khoản không hợp lệ!', 'warning');
        return;
    }
    
    var oldPass = ADMIN_LIST[idx].pass;
    ADMIN_LIST[idx].pass = newPass;
    
    // Nếu là admin hiện tại đang đăng nhập, cập nhật lại _SEC
    if (ADMIN_LIST[idx].user === _SEC.u()) {
        // Cập nhật pass chính trong ADMIN_PASS
        // Sửa trực tiếp ADMIN_LIST vì login dùng isAdmin()
    }
    
    // Clear fields
    document.getElementById('admin-new-password').value = '';
    document.getElementById('admin-confirm-password').value = '';
    sel.value = '';
    
    showToast('🔒 Đã đổi mật khẩu cho admin **' + ADMIN_LIST[idx].user + '** thành công!', 'success');
    
    // Reset session của admin bị đổi pass nếu đang login
    var currentUser = getCurrentUser();
    if (currentUser === ADMIN_LIST[idx].user && oldPass !== newPass) {
        // Admin tự đổi pass của chính mình → vẫn giữ session
        showToast('💡 Mật khẩu mới sẽ có hiệu lực ở lần đăng nhập sau.', 'info');
    }
}

// Select telco
function selectTelco(el) {
    selectedTelco = el.getAttribute('data-telco');
    var opts = document.querySelectorAll('#telco-select-group .telco-option');
    opts.forEach(function(o) { o.classList.remove('active'); });
    el.classList.add('active');
    updateCardRealAmount();
}

// Select denomination
function selectDenom(el) {
    selectedDenom = parseInt(el.getAttribute('data-denom'));
    var opts = document.querySelectorAll('#denom-select-group .denom-option');
    opts.forEach(function(o) { o.classList.remove('active'); });
    el.classList.add('active');
    updateCardRealAmount();
}

// Update real amount display
function updateCardRealAmount() {
    var realAmount = Math.floor(selectedDenom * CARD_CONFIG.DISCOUNT_RATE);
    var el = document.getElementById('card-real-amount');
    if (el) {
        el.textContent = new Intl.NumberFormat('vi-VN').format(realAmount) + 'đ';
    }
}

// Submit card
function submitCard() {
    var currentUser = getCurrentUser();
    if (!currentUser) {
        showToast('⚠️ Vui lòng đăng nhập để nạp thẻ!', 'warning');
        toggleModal('loginModal');
        return;
    }
    if (isAdminUser(currentUser)) {
        showToast('⚠️ Admin không thể nạp thẻ!', 'warning');
        return;
    }

    // ✅ RATE LIMIT - Chống spam nạp thẻ (1 lần/10 giây)
    if (!_RL.check('card_' + currentUser, 1, 10)) {
        showToast('⏳ Vui lòng đợi 10 giây trước khi nạp thẻ tiếp!', 'warning');
        return;
    }

    var cardCode = document.getElementById('card-code-input').value.trim();
    var cardSerial = document.getElementById('card-serial-input').value.trim();

    if (!cardCode || !cardSerial) {
        showToast('⚠️ Vui lòng nhập đầy đủ Mã thẻ và Số Seri!', 'warning');
        return;
    }
    if (cardCode.length < 5) {
        showToast('⚠️ Mã thẻ cào không hợp lệ!', 'warning');
        return;
    }
    if (cardSerial.length < 5) {
        showToast('⚠️ Số Seri không hợp lệ!', 'warning');
        return;
    }

    var realAmount = Math.floor(selectedDenom * CARD_CONFIG.DISCOUNT_RATE);
    var requests = getCardRequests();
    var newRequest = {
        id: 'card_' + Date.now() + '_' + Math.random().toString(36).substr(2, 6),
        username: currentUser,
        telco: selectedTelco,
        denom: selectedDenom,
        realAmount: realAmount,
        cardCode: cardCode,
        cardSerial: cardSerial,
        status: 'pending',
        createdAt: new Date().toISOString(),
        reviewedAt: null,
        reviewedBy: null,
        rejectReason: ''
    };

    requests.unshift(newRequest);
    saveCardRequests(requests);

    // Clear form
    document.getElementById('card-code-input').value = '';
    document.getElementById('card-serial-input').value = '';

    showToast('📱 Đã gửi thẻ cào! Admin sẽ duyệt trong 5-15 phút.', 'success');
    notifyCardSubmit(currentUser, selectedTelco, selectedDenom, realAmount, cardCode, cardSerial);
    renderCardForm();
}

// Render card form (wallet visibility, history)
function renderCardForm() {
    var currentUser = getCurrentUser();
    var walletSection = document.getElementById('wallet-section');
    if (!walletSection) return;

    if (currentUser && !isAdminUser(currentUser)) {
        walletSection.style.display = 'flex';
        var balance = getUserWallet(currentUser);
        var balEl = document.getElementById('wallet-balance-display');
        if (balEl) balEl.textContent = new Intl.NumberFormat('vi-VN').format(balance) + 'đ';
    } else {
        walletSection.style.display = 'none';
    }

    updateCardRealAmount();
}

// Render card history for a user
function renderCardHistory() {
    var currentUser = getCurrentUser();
    if (!currentUser) return;

    var history = getCardHistory(currentUser);
    var container = document.getElementById('card-history-list');
    if (!container) return;

    if (history.length === 0) {
        container.innerHTML = '<p style="text-align:center;color:var(--secondary-text);padding:20px">Chưa có giao dịch nào</p>';
        return;
    }

    var html = '';
    history.forEach(function(r) {
        var statusClass = r.status;
        var statusText = r.status === 'pending' ? '⏳ Chờ duyệt' : r.status === 'approved' ? '✅ Đã duyệt' : '❌ Từ chối';
        var timeStr = new Date(r.createdAt).toLocaleString('vi-VN');
        var rejectInfo = r.status === 'rejected' && r.rejectReason ? '<div style="font-size:11px;color:var(--danger);margin-top:4px">Lý do: ' + escapeHTML(r.rejectReason) + '</div>' : '';

        html += '<div class="history-item">' +
            '<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px">' +
            '<span class="history-amount">' + new Intl.NumberFormat('vi-VN').format(r.denom) + 'đ</span>' +
            '<span class="history-status ' + statusClass + '">' + statusText + '</span>' +
            '</div>' +
            '<div class="history-detail">📶 ' + r.telco.toUpperCase() + ' • 🎁 Nhận: ' + new Intl.NumberFormat('vi-VN').format(r.realAmount) + 'đ</div>' +
            '<div class="history-detail">🕐 ' + timeStr + '</div>' +
            rejectInfo +
            '</div>';
    });

    container.innerHTML = html;
}

// ==========================================
// ADMIN: CARD APPROVAL
// ==========================================
function renderAdminCardRequests() {
    var container = document.getElementById('admin-card-requests-list');
    if (!container) return;

    var pending = getPendingRequests();

    if (pending.length === 0) {
        container.innerHTML = '<p style="text-align:center;color:var(--secondary-text);padding:15px">✅ Không có đơn nạp thẻ nào chờ duyệt</p>';
        return;
    }

    var html = '';
    pending.forEach(function(r) {
        var timeStr = new Date(r.createdAt).toLocaleString('vi-VN');
        html += '<div class="card-request-item" id="card-req-' + r.id + '">' +
            '<div class="cr-header">' +
            '<span class="cr-user">👤 ' + escapeHTML(r.username) + '</span>' +
            '<span class="cr-amount">' + new Intl.NumberFormat('vi-VN').format(r.denom) + 'đ</span>' +
            '</div>' +
            '<div class="cr-info">' +
            '📶 <code>' + r.telco.toUpperCase() + '</code> • 🎁 Nhận: <code>' + new Intl.NumberFormat('vi-VN').format(r.realAmount) + 'đ</code><br>' +
            '🔢 Mã thẻ: <code>' + escapeHTML(r.cardCode) + '</code><br>' +
            '📋 Seri: <code>' + escapeHTML(r.cardSerial) + '</code><br>' +
            '🕐 ' + timeStr +
            '</div>' +
            '<div class="cr-actions">' +
            '<button class="btn-approve" data-approve-id="' + r.id + '">✅ Duyệt (+' + new Intl.NumberFormat('vi-VN').format(r.realAmount) + 'đ)</button>' +
            '<button class="btn-reject" data-reject-id="' + r.id + '">❌ Từ chối</button>' +
            '</div>' +
            '</div>';
    });

    container.innerHTML = html;

    // Attach event listeners (avoids inline onclick issues)
    var approveBtns = container.querySelectorAll('.btn-approve');
    var rejectBtns = container.querySelectorAll('.btn-reject');
    for (var i = 0; i < approveBtns.length; i++) {
        (function(btn) {
            btn.addEventListener('click', function() {
                approveCardRequest(this.getAttribute('data-approve-id'));
            });
        })(approveBtns[i]);
    }
    for (var j = 0; j < rejectBtns.length; j++) {
        (function(btn) {
            btn.addEventListener('click', function() {
                rejectCardRequest(this.getAttribute('data-reject-id'));
            });
        })(rejectBtns[j]);
    }
}

function approveCardRequest(requestId) {
    if (!confirm('Xác nhận DUYỆT đơn nạp thẻ này? Tiền sẽ được cộng vào ví khách hàng.')) return;

    var requests = getCardRequests();
    var found = requests.find(function(r) { return r.id === requestId; });

    if (!found || found.status !== 'pending') {
        showToast('⚠️ Đơn này đã được xử lý trước đó!', 'warning');
        renderAdminCardRequests();
        return;
    }

    found.status = 'approved';
    found.reviewedAt = new Date().toISOString();
    found.reviewedBy = getCurrentUser();
    saveCardRequests(requests);

    // Cộng tiền cho user
    addUserWallet(found.username, found.realAmount);

    showToast('✅ Đã duyệt đơn nạp thẻ! +' + new Intl.NumberFormat('vi-VN').format(found.realAmount) + 'đ cho ' + found.username, 'success');
    notifyAdminApproved(getCurrentUser(), found.username, found.realAmount);
    renderAdminCardRequests();

    // Update total
    var pendingCount = getPendingRequests().length;
    var badge = document.getElementById('admin-card-badge');
    if (badge) badge.textContent = pendingCount;
}

function rejectCardRequest(requestId) {
    var reason = prompt('Nhập lý do từ chối:');
    if (reason === null) return; // User cancelled
    if (!reason.trim()) {
        showToast('⚠️ Vui lòng nhập lý do từ chối!', 'warning');
        return;
    }

    var requests = getCardRequests();
    var found = requests.find(function(r) { return r.id === requestId; });

    if (!found || found.status !== 'pending') {
        showToast('⚠️ Đơn này đã được xử lý trước đó!', 'warning');
        renderAdminCardRequests();
        return;
    }

    found.status = 'rejected';
    found.reviewedAt = new Date().toISOString();
    found.reviewedBy = getCurrentUser();
    found.rejectReason = reason.trim();
    saveCardRequests(requests);

    showToast('❌ Đã từ chối đơn nạp thẻ của ' + found.username, 'warning');
    renderAdminCardRequests();

    var pendingCount = getPendingRequests().length;
    var badge = document.getElementById('admin-card-badge');
    if (badge) badge.textContent = pendingCount;
}

// Update card badge count
function updateCardBadge() {
    var pendingCount = getPendingRequests().length;
    var badge = document.getElementById('admin-card-badge');
    if (badge) {
        badge.textContent = pendingCount;
        badge.style.display = pendingCount > 0 ? 'inline' : 'none';
    }
}

// ==========================================
window.onload = function() {
    renderShopProducts();
    updateUIAuthState();
    renderCardForm();
    updateCardBadge();
    initOrderCheckbox();
};

</script>
<script type="module" src="https://static.cloudflareinsights.com/beacon.min.js/v4513226cdae34746b4dedf0b4dfa099e1781791509496" integrity="sha512-ZE9pZaUXND66v380QUtch/5sE9tPFh2zg45pR2PB0CVkCtOREv2AJKkSidISWkysEuQ0EH8faUU5du78bx87UQ==" data-cf-beacon='{"version":"2024.11.0","token":"3279d41ec7804124a29acc6e692f9a2b"}' crossorigin="anonymous"></script>
</body>
</html>
