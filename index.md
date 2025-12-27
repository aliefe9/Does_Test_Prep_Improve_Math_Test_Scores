<div id="viewer" style="max-width: 900px; margin: 0 auto;"></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/4.7.76/pdf.min.js"></script>
<script>
  const url = "{{ 'Ali_Does-Test-Prep-Improve-Math-Test-Scores-.pdf' | relative_url }}";
  const container = document.getElementById("viewer");

  pdfjsLib.GlobalWorkerOptions.workerSrc =
    "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/4.7.76/pdf.worker.min.js";

  (async () => {
    const pdf = await pdfjsLib.getDocument(url).promise;
    for (let pageNum = 1; pageNum <= pdf.numPages; pageNum++) {
      const page = await pdf.getPage(pageNum);
      const viewport = page.getViewport({ scale: 1.4 });

      const canvas = document.createElement("canvas");
      const context = canvas.getContext("2d");
      canvas.style.width = "100%";
      canvas.style.height = "auto";
      canvas.width = viewport.width;
      canvas.height = viewport.height;

      const wrapper = document.createElement("div");
      wrapper.style.margin = "24px 0";
      wrapper.style.boxShadow = "0 6px 24px rgba(0,0,0,0.12)";
      wrapper.style.borderRadius = "12px";
      wrapper.style.overflow = "hidden";
      wrapper.appendChild(canvas);

      container.appendChild(wrapper);

      await page.render({ canvasContext: context, viewport }).promise;
    }
  })();
</script>

<p><a href="Ali_Does-Test-Prep-Improve-Math-Test-Scores-.pdf">Download the PDF</a></p>
