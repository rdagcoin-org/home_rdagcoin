<template>
  <div class="home">
    <div class="canvas-container" ref="screenDom"></div>
    <div class="header">
      <div class="logoBox">
        <div class="logo"></div>
        <div class="name">
          <div class="h1">RDAG</div>
          <div class="tips">PWO COIN</div>
        </div>
      </div>
      <div class="menu">
        <a href="#" class="menuItem">HOME</a>
        <a href="#" class="menuItem">www</a>
        <a href="#" class="menuItem">eee</a>
      </div>
    </div>
    <div class="pages" ref="pages">
      <div class="page">
        <h2 class="title">RDAG: Safe Cryptocurrency</h2>
        <p class="contentText">A DAG cryptocurrency implemented using the RandomX algorithm</p>
        <br />
        <Card :color="['#ff0058', '#03a9f4']">
          <div class="cardContent">
            <p>Total Supply: 30 million coins</p>
            <p>Circulating supply: 26 million</p>
            <p>Block Time: 300 seconds</p>
            <p>POW Reward: 25 (RDAG)</p>
            <p>Mining Algorithm: RandomX</p>
          </div>
        </Card>
        <br />
        <span>proof-of-work (PoW) / CPU Mining / Community driven / Anonymous creator / Future-proof</span>
        <br />

        <div class="btn" style="--clr: #822155; width: 170px">
          <div>
            GitHub
            <el-icon><DArrowRight /></el-icon>
          </div>
          <i class="icon"></i>
        </div>
      </div>
      <div class="page">
        <h2 class="title">OUR WALLET</h2>

        <Card>
          <div class="cardContent">
            <div class="cardTitle">Electrum</div>
            <p>
              Fast and light wallet. It uses external servers to validate transactions and this makes it unnecessary to download the
              blockchain.
            </p>
            <p>Support: Windows, Linux, MacOS</p>
            <div class="btn" style="--clr: #4dff03">
              <div>
                Download
                <el-icon><DArrowRight /></el-icon>
              </div>
              <i class="icon"></i>
            </div>
          </div>
        </Card>
      </div>
      <div class="page">
        <h2 class="title">ThreeJS</h2>
        <p>start</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import * as THREE from 'three'
import { ref, onMounted } from 'vue'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'
import { DRACOLoader } from 'three/examples/jsm/loaders/DRACOLoader'
import { gsap } from 'gsap'
import Card from '@/com/card.vue'

let screenDom = ref(null)
let pages = ref(null)
onMounted(() => {
  let scene = new THREE.Scene()
  let camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 100000)

  camera.position.set(0, 0, 10)
  let renderer = new THREE.WebGLRenderer({ antialias: true })
  renderer.setSize(window.innerWidth, window.innerHeight)
  screenDom.value.appendChild(renderer.domElement)

  let url = './assets/25s.jpg'
  let envTexture = new THREE.TextureLoader().load(url)
  envTexture.mapping = THREE.EquirectangularReflectionMapping
  scene.background = envTexture
  scene.environment = envTexture

  function render() {
    requestAnimationFrame(render)
    renderer.render(scene, camera)
  }
  render()

  let light = new THREE.DirectionalLight(0xffffff, 1)
  light.position.set(0, 0, 1)
  scene.add(light)
  let light2 = new THREE.DirectionalLight(0xffffff, 0.5)
  light2.position.set(0, 0, -1)
  scene.add(light2)
  let light3 = new THREE.AmbientLight(0xffffff, 0.5)
  light3.position.set(-1, 1, 1)
  scene.add(light3)

  let dracoLoader = new DRACOLoader()
  dracoLoader.setDecoderPath('./draco/gltf/')
  dracoLoader.setDecoderConfig({ type: 'js' })
  let loader = new GLTFLoader()
  loader.setDRACOLoader(dracoLoader)
  loader.load('./model/ybbb2.glb', (gltf) => {
    gltf.scene.scale.set(80, 80, 80)
    gltf.scene.position.set(3, 0, 0)
    const model = gltf.scene
    scene.add(model)

    window.addEventListener('mousemove', (e) => {
      let x = (e.clientX / window.innerWidth) * 2 - 1
      let y = (e.clientY / window.innerHeight) * 2 - 1

      let timeline = gsap.timeline()
      timeline.to(gltf.scene.rotation, {
        duration: 0.5,
        x: y,
        y: x,
        duration: 1
      })
    })
  })

  loader.load('./model/22.glb', (gltf) => {
    gltf.scene.scale.set(50, 50, 50)
    gltf.scene.position.set(3, -8, 0)
    scene.add(gltf.scene)

    window.addEventListener('mousemove', (e) => {
      let x = (e.clientX / window.innerWidth) * 2 - 1
      let y = (e.clientY / window.innerHeight) * 2 - 1

      let timeline = gsap.timeline()
      timeline.to(gltf.scene.rotation, {
        duration: 0.5,
        x: y,
        y: x,
        duration: 1
      })
    })
  })

  loader.load('./model/gr75.glb', (gltf) => {
    // gltf.scene.scale.set(0.1, 0.1, 0.1);
    gltf.scene.position.set(3, -16, 0)
    scene.add(gltf.scene)

    window.addEventListener('mousemove', (e) => {
      let x = (e.clientX / window.innerWidth) * 2 - 1
      let y = (e.clientY / window.innerHeight) * 2 - 1

      let timeline = gsap.timeline()
      timeline.to(gltf.scene.rotation, {
        duration: 0.5,
        x: y,
        y: x,
        duration: 1
      })
    })
  })

  let page = 0
  let timeline2 = gsap.timeline()
  window.addEventListener('mousewheel', (e) => {
    if (e.wheelDelta < 0) {
      page++
      if (page > 2) {
        page = 2
      }
    }
    if (e.wheelDelta > 0) {
      page--
      if (page < 0) {
        page = 0
      }
    }
    if (!timeline2.isActive()) {
      timeline2.to(camera.position, {
        duration: 0.5,

        y: page * -8,
        duration: 1
      })
      gsap.to(pages.value, {
        duration: 1,
        y: -page * window.innerHeight,
        duration: 1
      })
    }
  })

  loader.load('./model/moon.glb', (gltf) => {
    let moon = gltf.scene.children[0]
    for (let j = 0; j < 10; j++) {
      let moonInstance = new THREE.InstancedMesh(moon.geometry, moon.material, 100)

      // scene.add(moon);
      for (let i = 0; i < 100; i++) {
        let x = Math.random() * 1000 - 500
        let y = Math.random() * 1000 - 500
        let z = Math.random() * 1000 - 500

        let matrix = new THREE.Matrix4()
        let size = Math.random() * 20 - 8
        matrix.makeScale(size, size, size)
        matrix.makeTranslation(x, y, z)
        moonInstance.setMatrixAt(i, matrix)
      }

      gsap.to(moonInstance.position, {
        duration: Math.random() * 10 + 2,
        z: -1000,
        ease: 'linear',
        repeat: -1
      })
      scene.add(moonInstance)
    }
  })
})
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}

body {
  background-color: #000;
}
.canvas-container {
  width: 100vw;
  height: 100vh;
}
.home {
  width: 100vw;
  height: 100vh;
  transform-origin: 0 0;
}

.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100px;
  display: flex;
  justify-content: space-between;
  align-items: center;

  .logoBox {
    display: flex;
    color: #fff;
    align-items: center;
    .logo {
      height: 120px;
      width: 120px;
      background-image: url('./assets/rdagcoin_logo.png');
      background-size: 60%;
      background-position: center;
      background-repeat: no-repeat;
    }
    .name {
      .h1 {
        font-size: 26px;
        font-weight: bold;
      }
      .tips {
        font-size: 12px;
        text-align: center;
      }
    }
  }
}

.canvas-container {
  width: 100%;
  height: 100%;
}
.menu {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-right: 50px;
}
.menuItem {
  padding: 0 15px;
  text-decoration: none;
  color: #fff;
  font-weight: 900;
  font-size: 15px;
}
.loading {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-image: url(./assets/loading.jpg);
  background-size: cover;
  filter: blur(50px);
  z-index: 100;
}
.progress {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 101;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  color: #fff;
  > img {
    padding: 0 15px;
  }
}

.pages {
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
}
.pages .page {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  color: #fff;
  padding: 15%;
  box-sizing: border-box;
}
.pages .page .title {
  font-size: 50px;
  font-weight: 900;
  margin-bottom: 20px;
}
.pages .page .contentText {
  font-size: 25px;
}

.btn {
  position: relative;
  background: transparent;
  color: #fff;
  text-decoration: none;
  font-size: 1.5em;
  letter-spacing: 0.1em;
  font-weight: 400;
  padding: 10px 30px;
  transition: 0.5s;
  cursor: pointer;
}

.btn:hover {
  letter-spacing: 0.25em;
  color: var(--clr);
  box-shadow: 0 0 35px var(--clr);
}

.btn::before {
  content: '';
  position: absolute;
  inset: 2px;
  background: transparent;
}

.btn div {
  position: relative;
  z-index: 1;
  white-space: nowrap;
}

.btn i.icon {
  position: absolute;
  inset: 0;
  display: block;
  opacity: 0.78;
}

.btn i.icon::before {
  content: '';
  position: absolute;
  top: -6px;
  left: 100%;
  transform: translateX(-50%);
  width: 10px;
  height: 10px;
  background: #27282c;
  border: 2px solid var(--clr);
  transition: 0.5s;
  opacity: 0;
}

.btn:hover i.icon::before {
  left: 0%;
  transform: translateX(-50%) rotate(45deg);
  box-shadow: 40px 39px var(--clr);
  opacity: 1;
}

.btn i.icon::after {
  content: '';
  position: absolute;
  bottom: -6px;
  left: 0;
  transform: translateX(-50%);
  width: 10px;
  height: 10px;
  background: #27282c;
  border: 2px solid var(--clr);
  transition: 0.5s;
  opacity: 0;
}

.btn:hover i.icon::after {
  left: 100%;
  transform: translateX(-50%) rotate(-45deg);
  box-shadow: 38px -39px var(--clr);
  opacity: 1;
}

.container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 40px 0;
  flex-wrap: wrap;
}

// .card:nth-child(2):after,
// .card:nth-child(2):before {
//   background: linear-gradient(315deg, #03a9f4, #ff0058);
// }

// .container .box:nth-child(3):after,
// .container .box:nth-child(3):before {
//   background: linear-gradient(315deg, #4dff03, #00d0ff);
// }

.cardContent {
  .cardTitle {
    font-size: 30px;
    font-weight: bold;
    margin-bottom: 20px;
  }
  p {
    font-size: 20px;
    margin-bottom: 14px;
  }
}
</style>
