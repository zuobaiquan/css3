
网页视口尺寸

window.screen.height  // 屏幕高度
window.innerHeight    // 网页视口高度

PC浏览器手机端模拟器的上 window.screen.height 和 window.innerHeight可能相同，因为没有浏览器地址栏及StatusBar状态栏的高度

document.body.clientHeight // body高度，根据body内容的高度为准

window.innerHeight == 100vh
window.innerWidth == 100vw


vh 网页视口高度的 1/100
vw 网页视口宽度的 1/100

vmax 取两者最大值，vmin 取两者最小值，一般 vh 比 vw 大，但如果手机屏幕横屏，vw 比 vh 大