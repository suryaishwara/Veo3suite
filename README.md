<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VEO3 MASTER PROMPT SUITE [PRO]</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #05070a; color: #f1f5f9; margin: 0; }
        .glass-panel { background: rgba(15, 23, 42, 0.7); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.05); }
        .input-box { background: rgba(2, 6, 23, 0.5); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 14px; padding: 14px; width: 100%; color: white; transition: 0.3s; font-size: 14px; }
        .input-box:focus { border-color: #6366f1; box-shadow: 0 0 0 4px rgba(99, 102, 241, 0.15); outline: none; }
        .btn-action { transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1); }
        .btn-action:active { transform: scale(0.95); }
        .scrollbar-custom::-webkit-scrollbar { width: 6px; }
        .scrollbar-custom::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 10px; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .animate-fade { animation: fadeIn 0.4s ease-out forwards; }
        option { background: #0f172a; color: white; }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect, useCallback } = React;

        const Icons = {
            Video: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z" /></svg>,
            User: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M18 9v3m0 0v3m0-3h3m-3 0h-3m-2-5a4 4 0 11-8 0 4 4 0 018 0zM3 20a6 6 0 0112 0v1H3v-1z" /></svg>,
            Camera: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" /><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" /></svg>,
            Music: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3" /></svg>,
            Plus: () => <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 4v16m8-8H4" /></svg>,
            Trash: () => <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg>,
            Lock: () => <svg className="w-5 h-5 text-indigo-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" /></svg>,
            Upload: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" /></svg>,
            Copy: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3" /></svg>,
            TikTok: () => <svg className="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1.04-.1z"/></svg>,
            Facebook: () => <svg className="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M22 12c0-5.52-4.48-10-10-10S2 6.48 2 12c0 4.84 3.44 8.87 8 9.8V15H8v-3h2V9.5C10 7.57 11.57 6 13.5 6H16v3h-2c-.55 0-1 .45-1 1v2h3l-.5 3h-2.5v6.8c4.56-.93 8-4.96 8-9.8z"/></svg>,
            Snack: () => <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" strokeWidth="2"><circle cx="12" cy="12" r="10"/><path d="M8 12h8m-4-4v8"/></svg> // Icon placeholder untuk SnackVideo
        };

        const config = {
            voices: ["Zephyr", "Puck", "Charon", "Kore", "Fenrir", "Leda", "Orus", "Aoede", "Callirrhoe"],
            angles: ["Eye Level", "Low Angle", "High Angle", "Bird's Eye", "Worm's Eye", "Dutch Angle", "POV", "Over the Shoulder"],
            vfx: ["None", "Lens Flare", "Motion Blur", "Film Grain", "Glitch", "Bloom", "Chromatic Aberration"],
            music: ["Cinematic Orchestral", "Cyberpunk Synth", "Dark Ambient", "Epic Trailer", "Lo-fi Chill"],
            languages: ["Indonesian", "English", "Japanese", "Korean", "Mandarin"]
        };

        // Komponen Secure Upload
        const SecureUpload = ({ onFile, currentImage, label, isAuthorized, onVerify }) => {
            const [pass, setPass] = useState("");
            if (!isAuthorized) {
                return (
                    <div className="h-full min-h-[140px] flex flex-col items-center justify-center bg-slate-900/50 rounded-2xl border-2 border-dashed border-indigo-500/30 p-4 text-center">
                        <Icons.Lock />
                        <p className="text-[9px] mt-2 text-slate-400 leading-tight">
                            Akses terbatas.<br/>Hubungi pembuat di <span className="text-indigo-400">081517101791</span> untuk kode.
                        </p>
                        <div className="mt-3 flex gap-1 w-full max-w-[120px]">
                            <input 
                                type="password" 
                                className="w-full bg-black/40 border border-slate-700 rounded-lg p-1 text-[10px] text-center text-white" 
                                placeholder="Pass"
                                value={pass}
                                onChange={(e) => setPass(e.target.value)}
                                onKeyDown={(e) => e.key === 'Enter' && onVerify(pass)}
                            />
                            <button onClick={() => onVerify(pass)} className="bg-indigo-600 px-2 py-1 rounded-lg text-[10px] font-bold text-white">OK</button>
                        </div>
                    </div>
                );
            }
            return (
                <label className="relative h-full min-h-[140px] flex flex-col items-center justify-center bg-slate-800/40 rounded-2xl border-2 border-dashed border-slate-700 cursor-pointer hover:bg-slate-800 transition-all overflow-hidden">
                    {currentImage ? (
                        <img src={currentImage} className="w-full h-full object-cover" />
                    ) : (
                        <div className="flex flex-col items-center opacity-40">
                            <Icons.Upload />
                            <span className="text-[10px] mt-2 font-bold uppercase">{label}</span>
                        </div>
                    )}
                    <input type="file" className="hidden" accept="image/*" onChange={onFile} />
                    {currentImage && (
                        <div className="absolute inset-0 bg-black/40 opacity-0 hover:opacity-100 flex items-center justify-center transition-opacity">
                            <span className="text-[10px] font-bold text-white uppercase tracking-widest">Ganti</span>
                        </div>
                    )}
                </label>
            );
        };

        const App = () => {
            const [step, setStep] = useState(0);
            const [isGenerating, setIsGenerating] = useState(false);
            const [output, setOutput] = useState(null);
            const [isAuthorized, setIsAuthorized] = useState(false);

            const [formData, setFormData] = useState({
                project: { model: 'veo-3', duration: '8s', ratio: '16:9', res: '4K', quality: 'Ultra' },
                characters: [{ id: Date.now(), name: 'Karakter 1', bio: '', outfit: '', expression: 'Netral', gesture: 'Idle', voice: 'Kore', photo: null }],
                scenes: [{ id: Date.now(), title: 'Adegan 1', start: '0', end: '4', activeChars: [], vfx: 'None', angle: 'Eye Level', visual_start: null, visual_end: null, desc: '' }],
                audio: { music: 'Cinematic Orchestral', ambient: '', volume: '80%' },
                dialogues: []
            });

            const steps = [
                { id: 'meta', title: 'Metadata', icon: <Icons.Video /> },
                { id: 'chars', title: 'Karakter', icon: <Icons.User /> },
                { id: 'scenes', title: 'Adegan', icon: <Icons.Video /> },
                { id: 'audio', title: 'Audio & Dialog', icon: <Icons.Music /> }
            ];

            const handleVerify = (pass) => {
                if (pass === "1791") {
                    setIsAuthorized(true);
                } else {
                    alert("Password Salah!");
                }
            };

            const handlePhoto = (id, type, subType, e) => {
                const file = e.target.files[0];
                if (!file) return;
                const reader = new FileReader();
                reader.onloadend = () => {
                    if (type === 'char') {
                        const updated = formData.characters.map(c => c.id === id ? { ...c, photo: reader.result } : c);
                        setFormData({ ...formData, characters: updated });
                    } else {
                        const updated = formData.scenes.map(s => s.id === id ? { ...s, [subType]: reader.result } : s);
                        setFormData({ ...formData, scenes: updated });
                    }
                };
                reader.readAsDataURL(file);
            };

            const generate = () => {
                setIsGenerating(true);
                setTimeout(() => {
                    const final = {
                        "production_header": {
                            "app": "VEO3 MASTER PROMPT SUITE",
                            "creator": "@surya.ishwara",
                            "tier": "PRO"
                        },
                        "metadata": formData.project,
                        "character_vault": formData.characters.map(c => ({
                            "name": c.name,
                            "visual_base": c.photo ? "Reference Provided" : "Text Prompt",
                            "bio": c.bio,
                            "wardrobe": c.outfit,
                            "acting": { "expression": c.expression, "gesture": c.gesture }
                        })),
                        "production_timeline": formData.scenes.map(s => ({
                            "scene": s.title,
                            "time": `${s.start}s - ${s.end}s`,
                            "cinematography": { "angle": s.angle, "vfx": s.vfx },
                            "visuals": { "has_start_anchor": s.visual_start ? true : false, "has_end_anchor": s.visual_end ? true : false },
                            "logic": s.desc
                        })),
                        "english_prompt": {
                            "summary": `PRO-Grade ${formData.project.quality} cinematic video using VEO3.`,
                            "instructions": formData.scenes.map(s => `[${s.title} @ ${s.start}s] ${s.angle}, ${s.vfx}: ${s.desc}`).join(". ")
                        },
                        "indonesian_prompt": {
                            "ringkasan": `Produksi sinematik PRO ${formData.project.quality} menggunakan VEO3.`,
                            "instruksi": formData.scenes.map(s => `[${s.title} @ ${s.start}s] Sudut ${s.angle}, Efek ${s.vfx}: ${s.desc}`).join(". ")
                        }
                    };
                    setOutput(final);
                    setIsGenerating(false);
                    setStep(steps.length);
                }, 1500);
            };

            return (
                <div className="min-h-screen p-4 md:p-8 flex flex-col items-center">
                    <div className="w-full max-w-3xl">
                        
                        {/* Header dengan Badge PRO */}
                        <header className="flex items-center justify-between mb-8 px-4">
                            <div className="flex items-center gap-4">
                                <div className="p-4 bg-indigo-600 rounded-2xl shadow-lg rotate-2">
                                    <Icons.Video />
                                </div>
                                <div>
                                    <div className="flex items-center gap-2">
                                        <h1 className="text-xl md:text-2xl font-black tracking-tight uppercase text-white leading-none">VEO3 MASTER PROMPT SUITE</h1>
                                        <span className="bg-indigo-500 text-[10px] font-black px-1.5 py-0.5 rounded text-white shadow-lg">PRO</span>
                                    </div>
                                    <p className="text-[10px] font-bold text-slate-500 tracking-[0.3em] uppercase mt-1">Advanced AI Video Architecture</p>
                                </div>
                            </div>
                        </header>

                        <div className="glass-panel rounded-[2.5rem] p-6 md:p-10 shadow-2xl relative min-h-[580px] flex flex-col">
                            
                            {isGenerating ? (
                                <div className="flex-1 flex flex-col items-center justify-center py-20">
                                    <div className="w-16 h-16 border-t-4 border-indigo-500 rounded-full animate-spin"></div>
                                    <p className="mt-6 font-bold text-indigo-400 animate-pulse tracking-widest uppercase">Membangun Proyek PRO...</p>
                                </div>
                            ) : step < steps.length ? (
                                <div className="animate-fade flex-1 flex flex-col">
                                    <div className="flex items-center justify-between mb-8">
                                        <div className="flex items-center gap-3">
                                            <span className="p-2 bg-indigo-500/20 rounded-xl text-indigo-400">{steps[step].icon}</span>
                                            <h2 className="text-xl font-bold text-white">{steps[step].title}</h2>
                                        </div>
                                        <div className="flex gap-1">
                                            {steps.map((_, i) => <div key={i} className={`h-1.5 rounded-full transition-all duration-500 ${step === i ? 'w-8 bg-indigo-500' : 'w-1.5 bg-slate-800'}`} />)}
                                        </div>
                                    </div>

                                    <div className="flex-1 overflow-y-auto scrollbar-custom pr-2 max-h-[400px] space-y-6">
                                        
                                        {step === 0 && (
                                            <div className="grid grid-cols-2 gap-4">
                                                <div className="col-span-2 bg-indigo-500/5 p-4 rounded-2xl border border-indigo-500/10">
                                                    <p className="text-[10px] font-bold text-indigo-400 mb-3 uppercase tracking-widest">Global Config</p>
                                                    <div className="grid grid-cols-2 gap-3">
                                                        <select className="input-box" onChange={e => setFormData({...formData, project: {...formData.project, ratio: e.target.value}})}>
                                                            <option>16:9 Landscape</option><option>9:16 Vertical</option><option>1:1 Square</option>
                                                        </select>
                                                        <select className="input-box" onChange={e => setFormData({...formData, project: {...formData.project, res: e.target.value}})}>
                                                            <option>4K Ultra HD</option><option>1080p Crystal</option>
                                                        </select>
                                                    </div>
                                                </div>
                                                <div className="col-span-2 space-y-1">
                                                    <label className="text-[10px] font-bold text-slate-500 uppercase ml-1">Total Durasi Proyek</label>
                                                    <input className="input-box font-bold text-indigo-400" value={formData.project.duration} onChange={e => setFormData({...formData, project: {...formData.project, duration: e.target.value}})} />
                                                </div>
                                            </div>
                                        )}

                                        {step === 1 && (
                                            <div className="space-y-6">
                                                <div className="flex justify-between items-center">
                                                    <p className="text-xs font-bold text-slate-500 uppercase tracking-widest">Library Karakter</p>
                                                    <button onClick={() => setFormData({...formData, characters: [...formData.characters, { id: Date.now(), name: `Char ${formData.characters.length+1}`, photo: null, bio: '', outfit: '', expression: 'Netral', gesture: 'Idle' }]})} className="text-[10px] font-black text-indigo-400 flex items-center gap-1 uppercase tracking-tighter hover:text-indigo-300 transition-colors"><Icons.Plus /> TAMBAH</button>
                                                </div>
                                                {formData.characters.map((c, i) => (
                                                    <div key={c.id} className="p-6 bg-slate-800/30 rounded-[2rem] border border-slate-700/50 space-y-4 mb-6 relative">
                                                        <button onClick={() => setFormData({...formData, characters: formData.characters.filter(char => char.id !== c.id)})} className="absolute top-4 right-4 text-slate-600 hover:text-red-500 transition-colors"><Icons.Trash /></button>
                                                        <div className="grid grid-cols-2 gap-4">
                                                            <div className="space-y-3">
                                                                <input className="input-box font-bold" placeholder="Nama Karakter" value={c.name} onChange={e => setFormData({...formData, characters: formData.characters.map(ch => ch.id === c.id ? {...ch, name: e.target.value} : ch)})}/>
                                                                <textarea className="input-box text-xs h-24" placeholder="Detail Biografi..." onChange={e => setFormData({...formData, characters: formData.characters.map(ch => ch.id === c.id ? {...ch, bio: e.target.value} : ch)})}/>
                                                            </div>
                                                            <SecureUpload 
                                                                label="Foto Dasar" 
                                                                currentImage={c.photo} 
                                                                isAuthorized={isAuthorized}
                                                                onVerify={handleVerify}
                                                                onFile={(e) => handlePhoto(c.id, 'char', null, e)} 
                                                            />
                                                        </div>
                                                        <div className="grid grid-cols-2 gap-2">
                                                            <input className="input-box text-xs" placeholder="Kostum" onChange={e => setFormData({...formData, characters: formData.characters.map(ch => ch.id === c.id ? {...ch, outfit: e.target.value} : ch)})}/>
                                                            <input className="input-box text-xs" placeholder="Gestur Utama" onChange={e => setFormData({...formData, characters: formData.characters.map(ch => ch.id === c.id ? {...ch, gesture: e.target.value} : ch)})}/>
                                                        </div>
                                                    </div>
                                                ))}
                                            </div>
                                        )}

                                        {step === 2 && (
                                            <div className="space-y-6">
                                                <div className="flex justify-between items-center">
                                                    <p className="text-xs font-bold text-slate-500 uppercase tracking-widest">Manager Adegan</p>
                                                    <button onClick={() => setFormData({...formData, scenes: [...formData.scenes, { id: Date.now(), title: `Adegan ${formData.scenes.length+1}`, start: '0', end: '4', activeChars: [], vfx: 'None', angle: 'Eye Level', visual_start: null, visual_end: null, desc: '' }]})} className="text-[10px] font-black text-indigo-400 flex items-center gap-1 uppercase tracking-tighter"><Icons.Plus /> TAMBAH ADEGAN</button>
                                                </div>
                                                {formData.scenes.map((s, i) => (
                                                    <div key={s.id} className="p-6 bg-slate-800/30 rounded-[2rem] border border-slate-700/50 space-y-4 mb-6">
                                                        <div className="flex justify-between items-center">
                                                            <input className="bg-transparent border-none text-indigo-400 font-bold outline-none text-lg" value={s.title} onChange={e => setFormData({...formData, scenes: formData.scenes.map(sc => sc.id === s.id ? {...sc, title: e.target.value} : sc)})}/>
                                                            <button onClick={() => setFormData({...formData, scenes: formData.scenes.filter(sc => sc.id !== s.id)})} className="text-slate-600 hover:text-red-500 transition-colors"><Icons.Trash /></button>
                                                        </div>
                                                        
                                                        <div className="grid grid-cols-2 gap-3">
                                                            <SecureUpload label="Frame Awal" currentImage={s.visual_start} isAuthorized={isAuthorized} onVerify={handleVerify} onFile={(e) => handlePhoto(s.id, 'scene', 'visual_start', e)} />
                                                            <SecureUpload label="Frame Akhir" currentImage={s.visual_end} isAuthorized={isAuthorized} onVerify={handleVerify} onFile={(e) => handlePhoto(s.id, 'scene', 'visual_end', e)} />
                                                        </div>

                                                        <div className="grid grid-cols-2 gap-2">
                                                            <div className="space-y-1"><label className="text-[10px] text-slate-500 font-bold uppercase">Mulai</label><input className="input-box text-xs" type="number" value={s.start} onChange={e => setFormData({...formData, scenes: formData.scenes.map(sc => sc.id === s.id ? {...sc, start: e.target.value} : sc)})}/></div>
                                                            <div className="space-y-1"><label className="text-[10px] text-slate-500 font-bold uppercase">Akhir</label><input className="input-box text-xs" type="number" value={s.end} onChange={e => setFormData({...formData, scenes: formData.scenes.map(sc => sc.id === s.id ? {...sc, end: e.target.value} : sc)})}/></div>
                                                        </div>
                                                        <select className="input-box text-xs" onChange={e => setFormData({...formData, scenes: formData.scenes.map(sc => sc.id === s.id ? {...sc, angle: e.target.value} : sc)})}>
                                                            {config.angles.map(a => <option key={a}>{a}</option>)}
                                                        </select>
                                                        <textarea className="input-box text-xs h-20" placeholder="Deskripsi aksi visual dalam adegan ini..." onChange={e => setFormData({...formData, scenes: formData.scenes.map(sc => sc.id === s.id ? {...sc, desc: e.target.value} : sc)})}/>
                                                    </div>
                                                ))}
                                            </div>
                                        )}

                                        {step === 3 && (
                                            <div className="space-y-6">
                                                <div className="bg-indigo-500/5 p-5 rounded-2xl border border-indigo-500/10 space-y-4">
                                                    <p className="text-[10px] font-bold text-indigo-400 uppercase tracking-widest">Audio Master</p>
                                                    <select className="input-box" onChange={e => setFormData({...formData, audio: {...formData.audio, music: e.target.value}})}>
                                                        {config.music.map(m => <option key={m}>{m}</option>)}
                                                    </select>
                                                    <input className="input-box" placeholder="Ambient Sound (e.g. City Noise)..." onChange={e => setFormData({...formData, audio: {...formData.audio, ambient: e.target.value}})}/>
                                                </div>
                                                <div className="space-y-4">
                                                    <p className="text-xs font-bold text-slate-500 uppercase tracking-widest">Timeline Dialog</p>
                                                    <button onClick={() => setFormData({...formData, dialogues: [...formData.dialogues, { id: Date.now(), charId: formData.characters[0].id, text: '', timestamp: '0', language: 'Indonesian', tone: 'Calm' }]})} className="text-[10px] font-black text-indigo-400 flex items-center gap-1 uppercase tracking-tighter transition-all"><Icons.Plus /> TAMBAH DIALOG</button>
                                                    {formData.dialogues.map(d => (
                                                        <div key={d.id} className="p-4 bg-slate-800/30 rounded-2xl border border-slate-700/50 space-y-2 relative">
                                                            <button onClick={() => setFormData({...formData, dialogues: formData.dialogues.filter(dg => dg.id !== d.id)})} className="absolute top-4 right-4 text-slate-600 hover:text-red-500 transition-colors"><Icons.Trash /></button>
                                                            <div className="grid grid-cols-2 gap-2">
                                                                <select className="input-box text-xs" onChange={e => setFormData({...formData, dialogues: formData.dialogues.map(dg => dg.id === d.id ? {...dg, charId: e.target.value} : dg)})}>
                                                                    {formData.characters.map(c => <option key={c.id} value={c.id}>{c.name}</option>)}
                                                                </select>
                                                                <input className="input-box text-xs" placeholder="Detik" type="number" onChange={e => setFormData({...formData, dialogues: formData.dialogues.map(dg => dg.id === d.id ? {...dg, timestamp: e.target.value} : dg)})}/>
                                                            </div>
                                                            <textarea className="input-box text-xs h-16" placeholder="Teks Dialog..." onChange={e => setFormData({...formData, dialogues: formData.dialogues.map(dg => dg.id === d.id ? {...dg, text: e.target.value} : dg)})}/>
                                                        </div>
                                                    ))}
                                                </div>
                                            </div>
                                        )}
                                    </div>

                                    <div className="mt-8 flex items-center justify-between">
                                        <button onClick={() => setStep(s => s - 1)} disabled={step === 0} className={`text-sm font-bold tracking-tight ${step === 0 ? 'opacity-0 pointer-events-none' : 'text-slate-500 hover:text-white transition-colors'}`}>KEMBALI</button>
                                        {step === steps.length - 1 ? (
                                            <button onClick={generate} className="bg-indigo-600 px-10 py-4 rounded-2xl font-black text-sm shadow-xl tracking-widest text-white uppercase active:scale-95 transition-all">FINALIZE</button>
                                        ) : (
                                            <button onClick={() => setStep(s => s + 1)} className="bg-slate-800 hover:bg-slate-700 px-8 py-4 rounded-2xl font-black text-sm border border-slate-700 tracking-widest text-white uppercase transition-all">LANJUTKAN</button>
                                        )}
                                    </div>
                                </div>
                            ) : (
                                <div className="animate-fade text-center flex-1 flex flex-col">
                                    <div className="flex justify-between items-center mb-6 px-2">
                                        <div className="text-left">
                                            <h2 className="text-xl font-black text-indigo-400 uppercase italic leading-none">PRO Architecture Ready</h2>
                                            <p className="text-[10px] font-bold text-slate-500 mt-1 uppercase">Generated for VEO3 Engine</p>
                                        </div>
                                        <button onClick={() => { navigator.clipboard.writeText(JSON.stringify(output, null, 2)); alert("JSON PRO Berhasil Disalin!"); }} className="p-3 bg-indigo-600 rounded-xl shadow-lg active:scale-90 transition-transform"><Icons.Copy /></button>
                                    </div>
                                    <div className="bg-slate-950 p-6 rounded-[2rem] border border-slate-800 text-left flex-1">
                                        <pre className="text-[10px] font-mono text-indigo-300 overflow-auto max-h-[350px] scrollbar-custom p-2 leading-relaxed">
                                            {JSON.stringify(output, null, 2)}
                                        </pre>
                                    </div>
                                    <button onClick={() => { setStep(0); setOutput(null); }} className="mt-8 w-full py-5 bg-slate-800 rounded-3xl font-black text-xs tracking-[0.3em] uppercase transition-all text-white">NEW PRODUCTION</button>
                                </div>
                            )}
                        </div>

                        {/* Footer / Credit */}
                        <footer className="mt-10 mb-6 flex flex-col items-center gap-4">
                            <div className="flex items-center gap-6">
                                <span className="text-slate-700 hover:text-white transition-colors cursor-pointer"><Icons.TikTok /></span>
                                <span className="text-slate-700 hover:text-blue-600 transition-colors cursor-pointer"><Icons.Facebook /></span>
                                <span className="text-slate-700 hover:text-orange-500 transition-colors cursor-pointer"><Icons.Snack /></span>
                            </div>
                            <p className="text-[10px] font-bold text-slate-600 uppercase tracking-[0.4em]">
                                by <span className="text-indigo-400 opacity-80">@surya.ishwara</span>
                            </p>
                        </footer>
                    </div>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>

