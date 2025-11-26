<!DOCTYPE html>
<html>
<head>
<title>Tabbed Browser</title>
<style>
body { margin:0; font-family:Arial, sans-serif; background:#111; color:#eee; }
#tabs { display:flex; background:#222; padding:5px; gap:5px; overflow-x:auto; }
.tab {
  padding:6px 10px;
  background:#444;
  border-radius:6px;
  cursor:pointer;
  white-space:nowrap;
}
.tab.active { background:#008cff; color:white; }
.tab .close {
  margin-left:6px;
  cursor:pointer;
  color:#ff8080;
}
#toolbar {
  display:flex; gap:5px; padding:10px; background:#222;
}
button { padding:8px; border:none; background:#444; color:white; border-radius:5px; cursor:pointer; }
input { flex:1; padding:8px; border-radius:5px; border:none; background:#333; color:white; }
iframe {
  width:100%;
  height:calc(100vh - 95px);
  border:none;
  background:white;
}
</style>
</head>
<body>

<div id="tabs"></div>

<div id="toolbar">
  <button onclick="goBack()">⟵</button>
  <button onclick="goForward()">⟶</button>
  <button onclick="reloadPage()">⟳</button>
  <input id="url" placeholder="Enter URL" />
  <button onclick="navigate()">Go</button>
  <button onclick="newTab()">+</button>
</div>

<div id="browser"></div>

<script>

let tabs = [];
let activeTab = -1;

function normalize(url){
  if (!url.startsWith("http")) return "https://" + url;
  return url;
}

function renderTabs(){
  const tabContainer = document.getElementById("tabs");
  tabContainer.innerHTML = "";

  tabs.forEach((tab, index) => {
    const t = document.createElement("div");
    t.className = "tab" + (index === activeTab ? " active" : "");
    t.innerHTML = `Tab ${index+1} <span class="close" onclick="closeTab(event, ${index})">✖</span>`;
    t.onclick = () => switchTab(index);
    tabContainer.appendChild(t);
  });
}

function newTab(url="https://example.com"){
  tabs.push({
    history: [url],
    index: 0,
    iframe: createIframe(url)
  });

  activeTab = tabs.length - 1;
  updateBrowser();
  renderTabs();
}

function createIframe(url){
  const frame = document.createElement("iframe");
  frame.src = url;
  return frame;
}

function updateBrowser(){
  const browser = document.getElementById("browser");
  browser.innerHTML = "";
  browser.appendChild(tabs[activeTab].iframe);
  document.getElementById("url").value = tabs[activeTab].iframe.src;
  renderTabs();
}

function switchTab(index){
  activeTab = index;
  updateBrowser();
}

function navigate(){
  const input = document.getElementById("url").value;
  const url = normalize(input);

  let tab = tabs[activeTab];
  tab.history.push(url);
  tab.index = tab.history.length - 1;
  tab.iframe.src = url;
}

function reloadPage(){
  tabs[activeTab].iframe.src = tabs[activeTab].iframe.src;
}

function goBack(){
  let tab = tabs[activeTab];
  if (tab.index > 0){
    tab.index--;
    tab.iframe.src = tab.history[tab.index];
    document.getElementById("url").value = tab.history[tab.index];
  }
}

function goForward(){
  let tab = tabs[activeTab];
  if (tab.index < tab.history.length - 1){
    tab.index++;
    tab.iframe.src = tab.history[tab.index];
    document.getElementById("url").value = tab.history[tab.index];
  }
}

function closeTab(event, index){
  event.stopPropagation(); // prevent switching tabs

  tabs.splice(index, 1);

  if (tabs.length === 0){
    newTab();
  } else if (activeTab >= tabs.length) {
    activeTab = tabs.length - 1;
  }

  updateBrowser();
  renderTabs();
}

// Start with one tab
newTab();

</script>
</body>
</html>
