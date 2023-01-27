<script lang="ts">
    import { onMount } from "svelte";
    import * as THREE from "three";
    import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
    import gsap from "gsap";

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
    const RADIUS = 30;
    const earth = new THREE.Mesh(
        new THREE.SphereGeometry(RADIUS, 50, 50, 20),
        new THREE.MeshPhongMaterial({
            map: textureLoader.load("./src/assets/earthmap1k.jpg"),
            bumpMap: textureLoader.load("./src/assets/earthbump1k.jpg"),
            bumpScale: 100,
            wireframe: true,
        })
    );
    scene.add(earth);

    const spherical = new THREE.Spherical();
    const nantes = {
        lon: THREE.Math.degToRad(2.349014),
        lat: THREE.Math.degToRad(90 - 48.864716),
    };

    const nantesVector = new THREE.Vector3().setFromSphericalCoords(
        RADIUS,
        nantes.lat,
        nantes.lon
    );
    var lineGeom = new THREE.BufferGeometry().setFromPoints([
        new THREE.Vector3(),
        nantesVector,
    ]);
    var line = new THREE.Line(
        lineGeom,
        new THREE.LineBasicMaterial({ color: "yellow" })
    );
    earth.add(line);

    onMount(() => {
        const renderer = new THREE.WebGLRenderer({
            canvas: document.querySelector("#canvas"),
            antialias: true,
        });
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.update();

        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.render(scene, camera);
        function animate() {
            requestAnimationFrame(animate);
            renderer.render(scene, camera);
            controls.update();
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(window.devicePixelRatio);
        }

        animate();
    });
</script>

<style>
    canvas {
        position: relative;
        top: 0;
        left: 0;
    }
</style>

<canvas id="canvas" />
