# Off_comic_website  
      // pixels per second, change this value to adjust movement speed
  
      /* Map */
      const MOVEMENT_SPEED = 100;
      const target = document.getElementById("target");

      const targetBoxes = [
        "target-box", "target-box2"
      ].map(id => document.getElementById(id));
      
      targetBoxes.forEach(box => box.addEventListener("click", mouseClicked));
      
      let animationRequestId = null;
      function mouseClicked(event) {
        // cancel running animation to run the newer animation
        if (animationRequestId) cancelAnimationFrame(animationRequestId);

        const computedTargetStyle = getComputedStyle(target);
        const startPosition = {
          x: parseFloat(computedTargetStyle.left),
          y: parseFloat(computedTargetStyle.top),
        };

        const destination = {
          x: event.clientX,
          y: event.clientY,
        };

        const distance = Math.sqrt(
          (destination.x - startPosition.x) ** 2 + (destination.y - startPosition.y) ** 2
        );

        const moveDuration = distance / MOVEMENT_SPEED;

        let startTime = document.timeline.currentTime;

        function animation(time) {
          const elapsedTime = (time - startTime) / 1000.0;

          // goes from 0 -> 1.0, once reaches 1.0 means the animation is completed
          const completionRate = Math.min(elapsedTime / moveDuration, 1.0);

          const newPosition = {
            x: startPosition.x + (destination.x - startPosition.x) * completionRate,
            y: startPosition.y + (destination.y - startPosition.y) * completionRate,
          };

          target.style.left = `${newPosition.x}px`;
          target.style.top = `${newPosition.y}px`;

          if (completionRate < 1.0) 
            animationRequestId = requestAnimationFrame(animation);
          else 
            animationRequestId = null;
        }

        animationRequestId = requestAnimationFrame(animation);
      }
