<script lang="ts">
    import { onMount } from "svelte";
    import * as THREE from "three";
    import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
    import gsap from "gsap";
    import { ScrollTrigger } from "gsap/ScrollTrigger"

    gsap.registerPlugin(ScrollTrigger);

    //Global scene
    const scene = new THREE.Scene();

    //Camera
    const camera = new THREE.PerspectiveCamera(
        70,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
    );
    camera.position.setZ(70);

    //Earth object

    scene.add(new THREE.AmbientLight(0xffffff));

    const textureLoader = new THREE.TextureLoader();

    const geometry = new THREE.SphereGeometry(30, 50, 50);
    const material = new THREE.MeshPhongMaterial({
        map: textureLoader.load("./src/assets/earthmap1k.jpg"),
        bumpMap: textureLoader.load("./src/assets/earthbump1k.jpg"),
        bumpScale: 100,
    });

    const earth = new THREE.Mesh(geometry, material);
    scene.add(earth);


    onMount(() => {
        const renderer = new THREE.WebGLRenderer({
            canvas: document.querySelector("#canvas"),
            antialias: true,
            alpha:true,
        });

        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);

        renderer.render(scene, camera);
        function animate() {
            requestAnimationFrame(animate);
            renderer.render(scene, camera);

            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(window.devicePixelRatio);
        }

        animate();
    });

    gsap.to(earth.rotation, {
        scrollTrigger: {
            trigger: "#canvas",
            start: "25% center",
            end: "center 100px",
            markers:true,
            scrub: 1,
            toggleActions: "restart pause reverse pause",
        },
        y : 5,
    })

    gsap.to(earth.position, {
        scrollTrigger: {
            trigger: "#canvas",
            start: "25% center",
            end: "center 100px",
            markers:true,
            scrub: 1,
            toggleActions: "restart pause reverse pause",
        },
        y : 50,
    })

</script>

<style>
    canvas {
        position:relative;
        top:0;
        left:0;
    }
</style>

<canvas id="canvas" />
