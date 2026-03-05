<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ARGOS TRIGGER HANGAR</title>
<link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@500;700&family=Inter:wght@300;400;500&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
body{margin:0;background:#0e1114;color:#e6e6e6;font-family:Inter,sans-serif}
body::before{content:"";position:fixed;inset:0;background-image:linear-gradient(rgba(255,255,255,.03) 1px,transparent 1px),linear-gradient(90deg,rgba(255,255,255,.03) 1px,transparent 1px);background-size:40px 40px;pointer-events:none}
header{font-family:Rajdhani;font-size:36px;letter-spacing:2px;padding:20px 30px;border-bottom:1px solid #333}
#addMech{margin:20px 10px 20px 30px;padding:10px 18px;background:#2a2f36;border:1px solid #555;color:white;font-family:Rajdhani;cursor:pointer}

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

.item{display:flex;justify-content:space-between;align-items:center;border:1px solid #333;padding:6px 8px;margin-top:4px;gap:8px}
.item input[type=number]{width:42px;background:#0f1318;border:1px solid #444;color:white;padding:2px 4px}

.destroyed{opacity:.45;border-color:#aa2b2b;position:relative}
.destroyed::after{content:"";position:absolute;left:0;right:0;top:50%;height:2px;background:#ff3c3c}

.imageBox{text-align:center;margin-bottom:10px}
.uploadBtn{margin-top:6px;padding:6px 10px;border:1px solid #666;background:#1a1f26;font-family:Rajdhani;cursor:pointer;color:white}
.imageBox img{max-width:100%;height:auto;cursor:pointer}
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
</style>
</head>
<body>
<header>ARGOS TRIGGER HANGAR</header>
<button id="addMech">Add Mech</button>
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
const template=document.getElementById("mechTemplate")

function saveHangar(){
localStorage.setItem("argosHangar",hangar.innerHTML)
}

function loadHangar(){
const data=localStorage.getItem("argosHangar")
if(data){hangar.innerHTML=data;restoreEvents()}
}

function updatePips(container,count){
const pips=[...container.children]
pips.forEach((p,i)=>p.classList.toggle("active",i<count))
container.dataset.value=count
saveHangar()
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
destroy.onclick=()=>{item.classList.toggle("destroyed");saveHangar()}

const del=document.createElement("div");del.className="itemDelete"
del.onclick=()=>{item.remove();saveHangar()}

right.appendChild(destroy)
right.appendChild(del)
item.appendChild(label)
item.appendChild(right)
list.appendChild(item)
saveHangar()
}

function setupImage(root){
const img=root.querySelector("img")
const input=root.querySelector("input[type=file]")
const btn=root.querySelector(".uploadBtn")
btn.onclick=()=>input.click()
input.onchange=e=>{
const file=e.target.files[0]
const reader=new FileReader()
reader.onload=x=>{img.src=x.target.result;img.style.display="block";btn.style.display="none";saveHangar()}
reader.readAsDataURL(file)
}
img.onclick=()=>input.click()
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
saveHangar()
}

const weaponList=root.querySelector(".weapons .list")
root.querySelector(".addWeapon").onclick=()=>addItem(weaponList,"Weapon")
root.querySelector(".addLimitedWeapon").onclick=()=>addItem(weaponList,"Limited Weapon",true)

const sysList=root.querySelector(".systems .list")
root.querySelector(".addSystem").onclick=()=>addItem(sysList,"System")
root.querySelector(".addLimited").onclick=()=>addItem(sysList,"Limited System",true)

setupImage(root)

hangar.appendChild(mech)
saveHangar()
}

function restoreEvents(){
document.querySelectorAll(".mech").forEach(root=>{
root.querySelector(".delete").onclick=()=>{root.remove();saveHangar()}

const core=root.querySelector(".coreBtn")
if(core)core.onclick=()=>{core.classList.toggle("charged");core.textContent=core.classList.contains("charged")?"Charged":"Expended";saveHangar()}

setupImage(root)

root.querySelectorAll(".destroyToggle").forEach(btn=>btn.onclick=()=>{btn.closest(".item").classList.toggle("destroyed");saveHangar()})
root.querySelectorAll(".itemDelete").forEach(btn=>btn.onclick=()=>{btn.closest(".item").remove();saveHangar()})

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

addMech.onclick=createMech



/* GLOBAL DELETE HANDLERS (ensures buttons work after restore) */
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

   saveHangar()
  }
  return
 }

 const itemDel=e.target.closest(".itemDelete")
 if(itemDel){
  const item=itemDel.closest(".item")
  if(item){
   item.remove()
   saveHangar()
  }
  return
 }

 const mechDel=e.target.closest(".delete")
 if(mechDel){
  const mech=mechDel.closest(".mech")
  if(mech){
   mech.remove()
   saveHangar()
  }
 }
})

loadHangar()
</script>

</body>
</html>
