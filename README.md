#add these codes in your html file

 1.   /* Floating iframe */
    #axis-viewer {
      position: absolute;
      top: 20px;
      right: 40px;
      width: 180px;
      height: 180px;
      border: none;
      border-radius: 12px;
      z-index: 10;
    }


2. <body>
  <iframe 
    id="axis-viewer"
    src="https://dishakulal24.github.io/threejs-axis-project/"
    loading="lazy"
    allowfullscreen>
  </iframe>




 3. window.addEventListener('message', (event) => {
      if (event.origin !== "https://dishakulal24.github.io") return;

      const { type, axis } = event.data;
      if (type === 'setCameraAxis') {
        rotateCameraToAxis(axis);
      }
    });

    //  Smooth camera transition to axis
    function rotateCameraToAxis(axis) {
      const radius = 1000;
      let targetPosition = new THREE.Vector3();

      switch (axis) {
        case 'x': targetPosition.set(radius, 0, 0); break;
        case '-x': targetPosition.set(-radius, 0, 0); break;
        case 'y': targetPosition.set(0, radius, 0); break;
        case '-y': targetPosition.set(0, -radius, 0); break;
        case 'z': targetPosition.set(0, 0, radius); break;
        case '-z': targetPosition.set(0, 0, -radius); break;
        default: return;
      }
      // Smooth animation
      const start = camera.position.clone();
      const end = targetPosition.clone();
      const duration = 500;
      const startTime = performance.now();

      function animateCamera(time) {
        const t = Math.min((time - startTime) / duration, 1);
        camera.position.lerpVectors(start, end, t);
        camera.lookAt(target);

        if (t < 1) requestAnimationFrame(animateCamera);
      }

      requestAnimationFrame(animateCamera);
    }
