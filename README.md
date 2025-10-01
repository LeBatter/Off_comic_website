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
 <!--
 <script>
    // Array of replacement images stored locally
    const replacementImages = [
      "./Assets_Comic_6/I_SEE_YOU.jpg"
    ];

    const imageElement = document.getElementById("mainImage");
    const originalSrc = imageElement.src;
    
    const imageElementFlash = document.getElementById("mainImageFlash");
    const randomIndex = Math.floor(Math.random() * replacementImages.length);
    imageElementFlash.src = replacementImages[randomIndex];

    // Only do this once, remove the "false &&" to actually remember the flash
    let hasFlashed = false && localStorage.getItem("imageFlashed");

    document.getElementById("testFlash").addEventListener("click", () => flashImage(null));

    document.querySelectorAll("a").forEach(a => a.addEventListener("click", (event) => {
        flashBeforeNavigate(event);
    }));

    addEventListener("beforeunload", (event) => { 
      flashBeforeNavigate(event);
    });

    function flashBeforeNavigate(event) {
      if (!hasFlashed) {
        hasFlashed = true;
        // Remember so it only happens once
        localStorage.setItem("imageFlashed", "true");
        flashImage(event);
      }
    }
    
    function flashImage(event) {
      event?.preventDefault();
      // Swap to random image
      imageElement.style.display = "none";
      imageElementFlash.style.display = "block";

      // Very short timeout to restore original
      setTimeout(() => {
        imageElement.src = originalSrc;
        imageElementFlash.style.display = "none";
        imageElement.style.display = "block";
        if (event?.target?.href) {
      window.location.href = event.target.href;
    }
  }, 2000); // 100 ms (0.1 sec)
    }
  </script>
  -->
<!--
<script>
  // Array of replacement images stored locally
  const replacementImages = [
    "./Assets_Comic_6/I_SEE_YOU.jpg"
  ];
  const redirectLink = "Page_1_interactable.html"; 
  const imageElement = document.getElementById("mainImage");
  const originalSrc = imageElement.src;

  let hasFlashed = false && localStorage.getItem("imageFlashed");

  document.getElementById("testFlash").addEventListener("click", () => flashImage(null));

  document.querySelectorAll("a").forEach(a =>
    a.addEventListener("click", (event) => {
      flashBeforeNavigate(event);
    })
  );

  addEventListener("beforeunload", (event) => {
    flashBeforeNavigate(event);
  });

  function flashBeforeNavigate(event) {
    if (!hasFlashed) {
      hasFlashed = true;
      localStorage.setItem("imageFlashed", "true");
      flashImage(event);
    }
  }

  function flashImage(event) {
    event?.preventDefault();

    // Pick a random replacement
    const randomIndex = Math.floor(Math.random() * replacementImages.length);
    const flashSrc = replacementImages[randomIndex];

    // Swap src directly
    imageElement.src = flashSrc;

    // Restore after delay
    setTimeout(() => {
      imageElement.src = originalSrc;

      // Resume navigation if needed
      if (event?.target?.href) {
        window.location.href = event.target.href;
      }
    }, 2000);
    //window.location.href = redirectLink;// flash duration in ms
  }
</script>
-->

<!--
<script>
  const replacementImages = [
    "./Assets_Comic_6/I_SEE_YOU.jpg"
  ];

  const redirectLink = "Page_1_interactable.html"; 
  const imageElement = document.getElementById("mainImage");
  const originalSrc = imageElement.src;

  let hasFlashed = localStorage.getItem("imageFlashed");

  // Flash button click
  document.getElementById("testFlash").addEventListener("click", () => flashImage());

  // Flash on all link clicks
  document.querySelectorAll("a").forEach(a =>
    a.addEventListener("click", (event) => flashBeforeNavigate(event))
  );

  function flashBeforeNavigate(event) {
    if (!hasFlashed) {
      hasFlashed = true;
      localStorage.setItem("imageFlashed", "true");
      flashImage(event);
    }
  }

  function flashImage(event) {
    // Prevent default navigation
    event?.preventDefault();

    // Pick a random flash image
    const randomIndex = Math.floor(Math.random() * replacementImages.length);
    const flashSrc = replacementImages[randomIndex];

    // Swap src to flash
    imageElement.src = flashSrc;

    // Delay navigation to show the flash
    setTimeout(() => {
      // Restore original image (optional)
      imageElement.src = originalSrc;

      // Navigate immediately to the target file
      window.location.href = redirectLink;
    }, 1000); // flash duration in ms
  }
</script>
-->
<!--
<script>
  const replacementImages = [
    "./Assets_Comic_6/I_SEE_YOU.jpg"
  ];
  const imageElement = document.getElementById("mainImage");
  const originalSrc = imageElement.src;

  const flashChance = 0.5; // 50% chance to flash
  let hasFlashed = false && localStorage.getItem("imageFlashed"); // remembers if flash already happened

  // Test Flash button
  document.getElementById("testFlash").addEventListener("click", () => flashImage());

  // Add flash logic to all links
  document.querySelectorAll("a").forEach(a => {
    a.addEventListener("click", (event) => {
      if (!hasFlashed && Math.random() < flashChance) {
        flashBeforeNavigate(event);
      }
      // else, link works normally
    });
  });

  function flashBeforeNavigate(event) {
    hasFlashed = true;
    localStorage.setItem("imageFlashed", "true");
    flashImage(event);
  }

  function flashImage(event) {
    // Prevent immediate navigation if triggered by a link
    event?.preventDefault();

    // Pick a random replacement image
    const randomIndex = Math.floor(Math.random() * replacementImages.length);
    imageElement.src = replacementImages[randomIndex];

    // Show flash for 1 second
    setTimeout(() => {
      imageElement.src = originalSrc; // restore original image

      // Navigate if this was triggered by a link
      if (event?.target?.href) {
        window.location.href = event.target.href;
      }
    }, 1000); // flash duration in ms
  }
</script>
-->