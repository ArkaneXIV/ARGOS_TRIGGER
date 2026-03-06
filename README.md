<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ARGOS TRIGGER HANGAR</title>
<link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@500;700&family=Inter:wght@300;400;500&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
body{margin:0;background:#0e1114;color:#e6e6e6;font-family:Inter,sans-serif}
body::before{content:"";position:fixed;inset:0;background-image:linear-gradient(rgba(255,255,255,.03) 1px,transparent 1px),linear-gradient(90deg,rgba(255,255,255,.03) 1px,transparent 1px);background-size:40px 40px;pointer-events:none}
header{font-family:Rajdhani;font-size:36px;letter-spacing:2px;padding:14px 30px;border-bottom:1px solid #333;display:flex;align-items:center;gap:16px}
#musicToggle{margin-left:auto;background:#2a2f36;border:1px solid #555;color:white;font-family:Rajdhani;padding:6px 12px;cursor:pointer}
#musicToggle:hover{border-color:#ff5a2b;box-shadow:0 0 10px rgba(255,80,30,.9)}
.headerEmblem{width:64px;height:64px;object-fit:contain;transition:filter .25s ease,transform .25s ease}
.headerEmblem:hover{filter:drop-shadow(0 0 8px rgba(255,90,43,.9)) drop-shadow(0 0 16px rgba(255,90,43,.6));transform:scale(1.05)}
#addMech{margin:20px 10px 20px 30px;padding:10px 18px;background:#2a2f36;border:1px solid #555;color:white;font-family:Rajdhani;cursor:pointer}
#saveData{margin:20px 10px;padding:10px 18px;background:#2a2f36;border:1px solid #555;color:white;font-family:Rajdhani;cursor:pointer;display:none}
#addMech:hover,#saveData:hover{border-color:#ff5a2b;box-shadow:0 0 10px rgba(255,80,30,.9)}
#saveData.unsavedGlow{border-color:#ff5a2b;box-shadow:0 0 12px rgba(255,80,30,1)}
#saveData.saving{border-color:#ffaa33;box-shadow:0 0 14px rgba(255,170,60,.9)}
#saveData.saved{border-color:#2ecc71;box-shadow:0 0 14px rgba(46,204,113,.9)}

#hangar{display:grid;grid-template-columns:repeat(auto-fill,minmax(420px,1fr));gap:20px;padding:20px}
.mech{background:linear-gradient(145deg,#1a1f26,#12161b);border:1px solid #333;padding:16px;position:relative;transition:.2s}
.mech:hover{box-shadow:0 0 15px rgba(255,80,30,.6)}

/* DELETE MECH BUTTON */
.fullRepairBtn{position:absolute;top:8px;right:40px;padding:2px 8px;font-family:Rajdhani;font-size:12px;border:1px solid #666;background:#2a2f36;color:white;cursor:pointer}
.fullRepairBtn.repairFlash{animation:repairGlow .6s ease}
@keyframes repairGlow{0%{box-shadow:0 0 0 rgba(255,120,40,0)}50%{box-shadow:0 0 14px rgba(255,120,40,.9)}100%{box-shadow:0 0 0 rgba(255,120,40,0)}}
.delete{position:absolute;top:8px;right:10px;width:20px;height:20px;border:1px solid #666;cursor:pointer;display:flex;align-items:center;justify-content:center}
.delete:before{content:"✕";font-size:12px;line-height:1;color:#aaa;pointer-events:none}

.mech h2 input{font-family:Rajdhani;font-size:20px;background:transparent;border:none;color:white;width:80%}
.section{margin-top:12px;border-top:1px solid #333;padding-top:8px}
label{font-size:12px;color:#aaa}
.stat{display:flex;gap:6px;align-items:center;margin-bottom:6px}
.stat input{width:50px;background:#0f1318;border:1px solid #444;color:white;padding:2px 4px}
.pips span{width:16px;height:16px;border:1px solid #666;display:inline-block;margin-right:3px;cursor:pointer}
.pips span.active{background:#ff5a2b}
button.small{background:#2a2f36;border:1px solid #555;color:white;padding:4px 8px;font-size:12px;cursor:pointer;margin-top:4px}
button.small:hover{border-color:#ff5a2b;box-shadow:0 0 6px rgba(255,80,30,.8)}

.item{display:flex;justify-content:space-between;align-items:center;border:1px solid #333;padding:6px 8px;margin-top:4px;gap:8px}
.item>div:last-child{margin-left:auto;display:flex;align-items:center;gap:6px}
.item input[type=number]{width:42px;background:#0f1318;border:1px solid #444;color:white;padding:2px 4px}

.destroyed{opacity:.45;border-color:#aa2b2b;position:relative}
.destroyed::after{content:"";position:absolute;left:0;right:0;top:50%;height:2px;background:#ff3c3c}

.imageBox{display:flex;justify-content:center;align-items:center;margin:0 auto 10px auto;width:500px;height:500px;max-width:100%;border:none;overflow:hidden;align-self:center}
.imageBox img{width:100%;height:100%;object-fit:cover;cursor:pointer;background:transparent;max-width:500px;max-height:500px}
.uploadBtn{margin-top:6px;padding:6px 10px;border:1px solid #666;background:#1a1f26;font-family:Rajdhani;cursor:pointer;color:white}

select{background:#0f1318;border:1px solid #444;color:white;padding:3px}

.coreBtn{padding:6px 10px;border:1px solid #555;background:#555;color:white;cursor:pointer}
.coreBtn.charged{background:#ff5a2b}

/* DESTROY TOGGLE (HOLLOW CIRCLE) */
.destroyToggle{width:18px;height:18px;border:2px solid #888;border-radius:50%;cursor:pointer;margin-left:10px}

/* DELETE ITEM BUTTON */
.itemDelete{width:18px;height:18px;border:1px solid #666;display:flex;align-items:center;justify-content:center;cursor:pointer;margin-left:12px}
.itemDelete:before{content:"✕";font-size:12px;line-height:1;color:#aaa;pointer-events:none}

/* HOVER HIGHLIGHT */
.destroyToggle:hover,.itemDelete:hover,.delete:hover,.fullRepairBtn:hover{border-color:#ff5a2b;box-shadow:0 0 6px rgba(255,80,30,.8)}

.terminal{font-family:'Share Tech Mono',monospace;position:relative;overflow:hidden}
.terminal::after{content:"";position:absolute;inset:0;background:linear-gradient(rgba(255,255,255,.04) 50%,transparent 50%);background-size:100% 4px;animation:scan 6s linear infinite;pointer-events:none}
@keyframes scan{0%{background-position:0 0}100%{background-position:0 200px}}
.viewMode .mech input,.viewMode .mech button,.viewMode .mech select,.viewMode .mech .destroyToggle,.viewMode .mech .itemDelete,.viewMode .mech .delete,.viewMode .mech .uploadBtn{pointer-events:none;opacity:.9}
.viewMode #addMech{display:none!important}
</style>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
</head>
<body>
<audio id="bgMusic" loop>
  <source src="audio/hangar_theme.mp3" type="audio/mpeg">
</audio>
<header>
<img class="headerEmblem" src="images/ARGOSemblem_White.png" alt="ARGOS Emblem">
ARGOS TRIGGER HANGAR

<div style="margin-left:auto;display:flex;align-items:center;gap:10px">
<button id="musicToggle">Music: Off</button>

<div id="authBox" style="display:flex;align-items:center;gap:6px">
<input id="passwordInput" type="password" placeholder="Password" style="background:#000;color:#fff;border:1px solid #333;padding:6px 8px;border-radius:4px;outline:none">
<button id="authorizeBtn" style="padding:6px 10px;background:#222;color:white;border:1px solid #444;border-radius:4px;cursor:pointer">Authorize</button>
</div>
</div>
</header>
<button id="addMech" style="display:none">Add Mech</button>
<button id="saveData">Save Data</button>
<div id="hangar"></div>

<template id="mechTemplate">
<div class="mech terminal">
<button class="fullRepairBtn">Full Repair</button>
<div class="delete"></div>

<div class="imageBox">
<img style="display:none">
<input type="file" style="display:none">
<button class="uploadBtn">Upload Image</button>
</div>

<h2><input value="New Mech"></h2>

<div class="section">
<div class="stat"><label>HP</label><input type="number" value="10"><span>/</span><input type="number" value="10"></div>
<div class="stat"><label>Heat</label><input type="number" value="0"><span>/</span><input type="number" value="6"></div>
<div class="stat"><label>Repairs</label><input type="number" value="3"><span>/</span><input type="number" value="3"></div>
</div>

<div class="section">
<label>Structure</label>
<div class="pips structure"></div>
<label>Stress</label>
<div class="pips stress"></div>
</div>

<div class="section">
<label>Core Power</label>
<button class="coreBtn charged">Charged</button>
</div>

<div class="section">
<label>Overcharge</label>
<select>
<option>+1</option>
<option>1d3</option>
<option>1d6</option>
<option>1d6+4</option>
</select>
</div>

<div class="section weapons">
<b>Weapons</b>
<div class="list"></div>
<button class="small addWeapon">Add Weapon</button>
<button class="small addLimitedWeapon">Add Limited Weapon</button>
</div>

<div class="section systems">
<b>Systems</b>
<div class="list"></div>
<button class="small addSystem">Add System</button>
<button class="small addLimited">Add Limited System</button>
</div>

</div>
</template>

<script>
const hangar=document.getElementById("hangar")

/* BACKGROUND MUSIC */
const bgMusic=document.getElementById("bgMusic")
const musicToggle=document.getElementById("musicToggle")
let musicStarted=false

if(bgMusic){
 bgMusic.volume=0.25

 const startMusic=()=>{
  if(!musicStarted){
   bgMusic.play().catch(()=>{})
   musicStarted=true
   if(musicToggle) musicToggle.textContent="Music: On"
  }
 }

 document.addEventListener("click",startMusic,{once:true})

 if(musicToggle){
  musicToggle.onclick=()=>{
   if(bgMusic.paused){
    bgMusic.play().catch(()=>{})
    musicToggle.textContent="Music: On"
   }else{
    bgMusic.pause()
    musicToggle.textContent="Music: Off"
   }
  }
 }
}

/* EDIT MODE AUTH (Supabase password check) */
const SUPABASE_URL="https://bnxxvbpjyuvjuqdjsxaw.supabase.co"
const SUPABASE_KEY="sb_publishable_7Es2Dkzgh3iKMuFGpiyFgw_RubTfAt_"
const sb=window.supabase.createClient(SUPABASE_URL,SUPABASE_KEY)

let editMode=false
let saving=false
let unsaved=false
let manualSave=false
document.body.classList.add("viewMode")

const addMechBtn=document.getElementById("addMech")
const passwordInput=document.getElementById("passwordInput")
const authorizeBtn=document.getElementById("authorizeBtn")

async function authorizePassword(){
 const pw=passwordInput.value
 if(!pw)return
 const enc=new TextEncoder().encode(pw)
 const buf=await crypto.subtle.digest("SHA-256",enc)
 const hash=[...new Uint8Array(buf)].map(b=>b.toString(16).padStart(2,"0")).join("")

 const {data}=await sb
  .from("settings")
  .select("value")
  .eq("key","edit_password_hash")
  .single()

 if(data && data.value===hash){
  editMode=true
  document.body.classList.remove("viewMode")
  addMechBtn.style.display="inline-block"
 document.getElementById("saveData").style.display="inline-block"

  authorizeBtn.textContent="Authorized"
  authorizeBtn.style.background="#1f8f3a"
  authorizeBtn.style.borderColor="#1f8f3a"
  authorizeBtn.style.color="#fff"
  authorizeBtn.disabled=true
  passwordInput.disabled=true
}else{
  alert("Incorrect password")
 }
}

authorizeBtn.onclick=authorizePassword
passwordInput.addEventListener("keydown",e=>{if(e.key==="Enter"){authorizePassword()}})
const template=document.getElementById("mechTemplate")

function markUnsaved(){
 if(!editMode)return
 unsaved=true
 const btn=document.getElementById("saveData")
 if(btn)btn.classList.add("unsavedGlow")
}

async function saveHangar(){
 if(!editMode || !manualSave) return
 if(saving) return
 saving = true

 const btn = document.getElementById("saveData")
 if(btn){
  btn.classList.remove("unsavedGlow","saved")
  btn.classList.add("saving")
  btn.textContent = "Saving..."
 }

 try{
  // Ensure form values persist in saved HTML
  const clone = hangar.cloneNode(true)

  clone.querySelectorAll('input').forEach(i=>{
   if(i.type==='number' || i.type==='text') i.setAttribute('value',i.value)
  })

  clone.querySelectorAll('select').forEach(s=>{
   s.querySelectorAll('option').forEach(o=>o.removeAttribute('selected'))
   const opt=[...s.options].find(o=>o.value===s.value || o.text===s.value)
   if(opt) opt.setAttribute('selected','selected')
  })

  const html = clone.innerHTML

  await sb
   .from("hangar")
   .update({ data: html })
   .eq("id","main")

  manualSave = false
  unsaved = false

  if(btn){
   btn.classList.remove("saving")
   btn.classList.add("saved")
   btn.textContent = "Saved"
  }

  setTimeout(()=>{
   if(btn){
    btn.classList.remove("saved")
    btn.textContent = "Save Data"
   }
   saving = false
  },1200)

 }catch(e){
  console.error(e)
  if(btn){
   btn.classList.remove("saving")
   btn.textContent = "Save Failed"
  }
  saving = false
 }
}

async function loadHangar(){
 const { data } = await sb
  .from("hangar")
  .select("data")
  .eq("id","main")
  .single()

 if(data && data.data){
  hangar.innerHTML = data.data
  restoreEvents()
 }
}

/* REALTIME VIEWER SYNC */
sb.channel('hangar-live')
 .on('postgres_changes',{event:'UPDATE',schema:'public',table:'hangar'},payload=>{
  // Prevent editor from overwriting their own local state
  if(editMode) return
  if(payload.new && payload.new.data){
   hangar.innerHTML = payload.new.data
   restoreEvents()
  }
 })
 .subscribe()

function updatePips(container,count){
const pips=[...container.children]
pips.forEach((p,i)=>p.classList.toggle("active",i<count))
container.dataset.value=count
markUnsaved();saveHangar()
}

function createPips(container,max=4){
container.dataset.value=0
for(let i=0;i<max;i++){container.appendChild(document.createElement("span"))}
container.addEventListener("click",()=>{
let v=parseInt(container.dataset.value);if(v<max)v++;updatePips(container,v)
})
container.addEventListener("contextmenu",e=>{
e.preventDefault();let v=parseInt(container.dataset.value);if(v>0)v--;updatePips(container,v)
})
}

function enableDrag(list){
let dragged=null
list.querySelectorAll('.item').forEach(i=>{
 i.draggable=true
 i.addEventListener('dragstart',()=>{dragged=i})
 i.addEventListener('dragover',e=>{e.preventDefault();const after=i.getBoundingClientRect().top+i.offsetHeight/2;if(e.clientY<after){list.insertBefore(dragged,i)}else{list.insertBefore(dragged,i.nextSibling)}})
 i.addEventListener('drop',()=>{markUnsaved();saveHangar()})
})
}

function addItem(list,name,limited=false){
const item=document.createElement("div");item.className="item"
const label=document.createElement("input");label.value=name
label.style.background="transparent";label.style.border="none";label.style.color="white"
const right=document.createElement("div");right.style.display="flex";right.style.alignItems="center";right.style.gap="6px"

if(limited){
const cur=document.createElement("input");cur.type="number";cur.value=0
const max=document.createElement("input");max.type="number";max.value=3
right.appendChild(cur);right.appendChild(document.createTextNode("/"));right.appendChild(max)
}

const destroy=document.createElement("div");destroy.className="destroyToggle"
destroy.onclick=()=>{item.classList.toggle("destroyed");markUnsaved();saveHangar()}

const del=document.createElement("div");del.className="itemDelete"
del.onclick=()=>{item.remove();markUnsaved();saveHangar()}

right.appendChild(destroy)
right.appendChild(del)
item.appendChild(label)
item.appendChild(right)
list.appendChild(item)
enableDrag(list)
markUnsaved();saveHangar()
}

function setupImage(root){
const img=root.querySelector("img")
const input=root.querySelector("input[type=file]")
const btn=root.querySelector(".uploadBtn")

function openFile(){ if(!editMode) return; input.click() }

btn.onclick=openFile

input.onchange=e=>{
 if(!editMode) return
 const file=e.target.files[0]
 const reader=new FileReader()
 reader.onload=x=>{
  img.src=x.target.result
  img.style.display="block"
  btn.style.display="none"
  markUnsaved();saveHangar()
 }
 reader.readAsDataURL(file)
}

img.onclick=openFile
}

function createMech(){
const mech=template.content.cloneNode(true)
const root=mech.querySelector(".mech")



createPips(root.querySelector(".structure"))
createPips(root.querySelector(".stress"))

const core=root.querySelector(".coreBtn")
core.onclick=()=>{
if(core.classList.contains("charged")){core.classList.remove("charged");core.textContent="Expended"}
else{core.classList.add("charged");core.textContent="Charged"}
markUnsaved();saveHangar()
}

const weaponList=root.querySelector(".weapons .list")
root.querySelector(".addWeapon").onclick=()=>addItem(weaponList,"Weapon")
root.querySelector(".addLimitedWeapon").onclick=()=>addItem(weaponList,"Limited Weapon",true)

const sysList=root.querySelector(".systems .list")
root.querySelector(".addSystem").onclick=()=>addItem(sysList,"System")
root.querySelector(".addLimited").onclick=()=>addItem(sysList,"Limited System",true)

setupImage(root)

hangar.appendChild(mech)
markUnsaved();saveHangar()
}

function restoreEvents(){
document.querySelectorAll(".mech").forEach(root=>{
root.querySelector(".delete").onclick=()=>{
 if(!editMode) return
 if(!confirm("Delete this mech?")) return
 root.remove();markUnsaved();saveHangar()
}

const core=root.querySelector(".coreBtn")
if(core)core.onclick=()=>{core.classList.toggle("charged");core.textContent=core.classList.contains("charged")?"Charged":"Expended";markUnsaved();saveHangar()}

setupImage(root)

root.querySelectorAll(".destroyToggle").forEach(btn=>btn.onclick=()=>{btn.closest(".item").classList.toggle("destroyed");markUnsaved();saveHangar()})
enableDrag(root.querySelector('.weapons .list'))
enableDrag(root.querySelector('.systems .list'))

root.querySelectorAll(".itemDelete").forEach(btn=>btn.onclick=()=>{btn.closest(".item").remove();markUnsaved();saveHangar()})

root.querySelectorAll(".pips").forEach(container=>{
container.addEventListener("click",()=>{let v=parseInt(container.dataset.value||0);if(v<4)v++;updatePips(container,v)})
container.addEventListener("contextmenu",e=>{e.preventDefault();let v=parseInt(container.dataset.value||0);if(v>0)v--;updatePips(container,v)})
})

const weaponList=root.querySelector(".weapons .list")
root.querySelector(".addWeapon").onclick=()=>addItem(weaponList,"Weapon")
root.querySelector(".addLimitedWeapon").onclick=()=>addItem(weaponList,"Limited Weapon",true)

const sysList=root.querySelector(".systems .list")
root.querySelector(".addSystem").onclick=()=>addItem(sysList,"System")
root.querySelector(".addLimited").onclick=()=>addItem(sysList,"Limited System",true)
})
}

document.getElementById("saveData").onclick=()=>{manualSave=true;markUnsaved();saveHangar()}

addMech.onclick=()=>{if(!editMode)return;createMech()}



/* GLOBAL DELETE HANDLERS (ensures buttons work after restore) */
/* TRACK INPUT CHANGES SO ALL DATA SAVES */
hangar.addEventListener("input",e=>{ if(editMode){ markUnsaved(); } })
hangar.addEventListener("change",e=>{ if(editMode){ markUnsaved(); } })

hangar.addEventListener("click",e=>{
 const repair=e.target.closest(".fullRepairBtn")
 if(repair){
  const mech=repair.closest(".mech")
  if(mech){
   const stats=mech.querySelectorAll(".stat")
   if(stats[0]){const cur=stats[0].children[1];const max=stats[0].children[3];cur.value=max.value}
   if(stats[1]){const cur=stats[1].children[1];cur.value=0}
   if(stats[2]){const cur=stats[2].children[1];const max=stats[2].children[3];cur.value=max.value}

   mech.querySelectorAll(".item").forEach(item=>{
    item.classList.remove("destroyed")
    const nums=item.querySelectorAll("input[type=number]")
    if(nums.length===2){nums[0].value=nums[1].value}
   })

   mech.querySelectorAll(".pips").forEach(p=>{
    const spans=[...p.children]
    p.dataset.value=4
    spans.forEach((s,i)=>s.classList.toggle("active",i<4))
   })

   const core=mech.querySelector(".coreBtn")
   if(core){core.classList.add("charged");core.textContent="Charged"}

   const over=mech.querySelector("select")
   if(over)over.value="+1"

   repair.classList.remove("repairFlash")
   void repair.offsetWidth
   repair.classList.add("repairFlash")

   markUnsaved();saveHangar()
  }
  return
 }

 const itemDel=e.target.closest(".itemDelete")
 if(itemDel){
  const item=itemDel.closest(".item")
  if(item){
   item.remove()
   markUnsaved();saveHangar()
  }
  return
 }

 const mechDel=e.target.closest(".delete")
 if(mechDel){
  if(!editMode) return
  const mech=mechDel.closest(".mech")
  if(mech){
   if(!confirm("Delete this mech?")) return
   mech.remove()
   markUnsaved();saveHangar()
  }
 }
})

loadHangar()
</script>
