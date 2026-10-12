<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>AI Plant Disease Detector</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Segoe UI', Roboto, system-ui, -apple-system, sans-serif;
      background: #f0f7f0;
      display: flex;
      justify-content: center;
      min-height: 100vh;
      margin: 0;
      padding: 12px;
    }

    .app-container {
      max-width: 520px;
      width: 100%;
      margin: 0 auto;
    }

    .card {
      background: white;
      border-radius: 32px;
      padding: 20px 18px;
      box-shadow: 0 12px 28px rgba(0, 40, 0, 0.08);
      margin-bottom: 20px;
      transition: box-shadow 0.2s;
    }

    .card-header {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 18px;
    }

    .card-header h2 {
      font-size: 1.4rem;
      font-weight: 600;
      color: #1f4f1f;
      letter-spacing: -0.02em;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .leaf-icon {
      font-size: 1.8rem;
      line-height: 1;
    }

    .api-section {
      background: #e9f3e9;
      border-radius: 24px;
      padding: 16px;
      margin-bottom: 20px;
    }

    .api-label {
      font-weight: 600;
      font-size: 0.9rem;
      color: #2a5a2a;
      margin-bottom: 8px;
      display: block;
    }

    .api-input-wrapper {
      display: flex;
      gap: 8px;
      align-items: center;
    }

    .api-input-wrapper input {
      flex: 1;
      padding: 14px 16px;
      border: 2px solid #c8e0c8;
      border-radius: 40px;
      font-size: 0.95rem;
      background: white;
      outline: none;
      transition: border 0.2s;
      font-family: inherit;
      min-width: 0;
    }

    .api-input-wrapper input:focus {
      border-color: #2e7d32;
    }

    .toggle-btn {
      background: white;
      border: 2px solid #c8e0c8;
      border-radius: 40px;
      width: 52px;
      height: 52px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.3rem;
      cursor: pointer;
      color: #2a5a2a;
      transition: background 0.2s;
      flex-shrink: 0;
      font-family: inherit;
    }

    .toggle-btn:hover {
      background: #e0f0e0;
    }

    .save-key {
      margin-top: 12px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .save-hint {
      font-size: 0.75rem;
      color: #4a6e4a;
    }

    .btn {
      border: none;
      border-radius: 60px;
      padding: 16px 24px;
      font-weight: 600;
      font-size: 1.05rem;
      cursor: pointer;
      transition: all 0.2s;
      font-family: inherit;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      letter-spacing: -0.01em;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
    }

    .btn-primary {
      background: #2e7d32;
      color: white;
      width: 100%;
      box-shadow: 0 8px 18px rgba(46, 125, 50, 0.25);
    }

    .btn-primary:active {
      transform: scale(0.98);
      background: #1e5e22;
    }

    .btn-primary:disabled {
      opacity: 0.6;
      transform: none;
      box-shadow: none;
      pointer-events: none;
    }

    .btn-secondary {
      background: white;
      color: #2e7d32;
      border: 2px solid #b8d9b8;
      padding: 12px 20px;
    }

    .camera-container {
      border-radius: 28px;
      overflow: hidden;
      background: #0f1f0f;
      margin-bottom: 16px;
      position: relative;
      aspect-ratio: 1 / 1;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    #cameraVideo {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
      background: #1a2a1a;
    }

    .camera-placeholder {
      color: #a0c0a0;
      font-size: 0.95rem;
      text-align: center;
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }

    .camera-placeholder span {
      font-size: 3rem;
    }

    .capture-row {
      display: flex;
      gap: 12px;
      margin-bottom: 12px;
      flex-wrap: wrap;
    }

    .capture-row .btn {
      flex: 1;
      padding: 16px 12px;
    }

    .file-upload-label {
      background: white;
      border: 2px dashed #b8d9b8;
      border-radius: 60px;
      padding: 16px 12px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      font-weight: 500;
      color: #2a5a2a;
      cursor: pointer;
      flex: 1;
      font-size: 0.95rem;
      transition: background 0.2s;
      text-align: center;
      font-family: inherit;
    }

    .file-upload-label:hover {
      background: #f0faf0;
    }

    input[type="file"] {
      display: none;
    }

    .preview-section {
      margin-top: 16px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .preview-label {
      font-size: 0.8rem;
      text-transform: uppercase;
      letter-spacing: 0.03em;
      color: #5a7a5a;
      margin-bottom: 8px;
      font-weight: 600;
    }

    #previewCanvas {
      max-width: 100%;
      border-radius: 24px;
      background: #e0ede0;
      border: 2px solid #b8d9b8;
      max-height: 200px;
      object-fit: contain;
    }

    .loading-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 12px;
      margin: 16px 0;
    }

    .spinner {
      width: 40px;
      height: 40px;
      border: 4px solid #d0e8d0;
      border-top: 4px solid #2e7d32;
      border-radius: 50%;
      animation: spin 1s linear infinite;
    }

    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .status-text {
      font-size: 0.95rem;
      color: #2a5a2a;
      font-weight: 500;
      text-align: center;
    }

    .error-banner {
      background: #ffebee;
      border-left: 6px solid #c62828;
      color: #b71c1c;
      padding: 14px 16px;
      border-radius: 18px;
      font-size: 0.9rem;
      font-weight: 500;
      margin: 16px 0 0 0;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .diagnostic-card {
      background: #f7fff7;
      border-radius: 28px;
      padding: 20px;
      border: 2px solid #d0e8d0;
      margin-top: 16px;
    }

    .plant-header {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 10px;
      margin-bottom: 16px;
    }

    .plant-name {
      font-size: 1.6rem;
      font-weight: 700;
      color: #1f3f1f;
      line-height: 1.2;
    }

    .status-badge {
      padding: 6px 16px;
      border-radius: 40px;
      font-weight: 600;
      font-size: 0.85rem;
      letter-spacing: 0.01em;
      background: #e0f0e0;
      color: #1f4f1f;
    }

    .status-badge.healthy {
      background: #d4edda;
      color: #0b5e0b;
    }

    .status-badge.diseased {
      background: #ffe0b0;
      color: #a13000;
    }

    .detail-row {
      margin-bottom: 14px;
      border-bottom: 1px solid #e0f0e0;
      padding-bottom: 12px;
    }

    .detail-row:last-child {
      border-bottom: none;
      margin-bottom: 0;
      padding-bottom: 0;
    }

    .detail-label {
      font-size: 0.75rem;
      text-transform: uppercase;
      letter-spacing: 0.04em;
      color: #4a6e4a;
      font-weight: 700;
      margin-bottom: 6px;
    }

    .detail-value {
      font-size: 1rem;
      color: #1a2e1a;
      line-height: 1.45;
      font-weight: 500;
    }

    .cure-box {
      background: #f0faf0;
      border-radius: 18px;
      padding: 14px;
      margin-top: 8px;
    }

    .cure-box .detail-label {
      color: #2e7d32;
    }

    .confidence-pill {
      display: inline-block;
      background: #e8f0e8;
      padding: 4px 12px;
      border-radius: 30px;
      font-size: 0.8rem;
      font-weight: 600;
      color: #2a5a2a;
      margin-left: 8px;
    }

    .hidden {
      display: none !important;
    }

    .small-note {
      font-size: 0.7rem;
      color: #6a8a6a;
      margin-top: 6px;
    }
  </style>
</head>
<body>
<div class="app-container">
  <div class="card">
    <div class="card-header">
      <h2><span class="leaf-icon">🌿</span> AI Plant Doctor</h2>
    </div>
    <div class="api-section">
      <label class="api-label" for="apiKeyInput">Google Gemini API Key</label>
      <div class="api-input-wrapper">
        <input type="password" id="apiKeyInput" placeholder="Paste your API key here..." autocomplete="off" spellcheck="false">
        <button class="toggle-btn" id="toggleApiKeyBtn" title="Show/hide key" type="button">👁️</button>
      </div>
      <div class="save-key">
        <span class="save-hint">🔒 Saved locally in your browser</span>
        <button class="btn btn-secondary" style="padding: 8px 18px; font-size: 0.85rem;" id="saveKeyBtn" type="button">Save</button>
      </div>
    </div>
    <div class="small-note">Your key never leaves your device except to call Gemini API.</div>
  </div>

  <div class="card">
    <div class="camera-container" id="cameraContainer">
      <video id="cameraVideo" autoplay playsinline muted></video>
      <div class="camera-placeholder" id="cameraPlaceholder">
        <span>📷</span>
        <p>Camera preview will appear here</p>
      </div>
    </div>

    <div class="capture-row">
      <button class="btn btn-primary" id="captureBtn" type="button" disabled>📸 Capture &amp; Analyze</button>
    </div>
    <div class="capture-row">
      <label class="file-upload-label" for="fileInput">
        📁 Upload photo
      </label>
      <input type="file" id="fileInput" accept="image/*" capture="environment">
    </div>

    <div class="preview-section hidden" id="previewSection">
      <div class="preview-label">Captured leaf</div>
      <canvas id="previewCanvas" width="300" height="300"></canvas>
    </div>

    <div class="loading-container hidden" id="loadingContainer">
      <div class="spinner"></div>
      <div class="status-text" id="statusText">Scanning leaf structure...</div>
    </div>

    <div class="error-banner hidden" id="errorBanner">
      <span>⚠️</span>
      <span id="errorMessage">Something went wrong.</span>
    </div>
  </div>

  <div class="card hidden" id="resultCard">
    <div class="plant-header">
      <span class="plant-name" id="plantName">—</span>
      <span class="status-badge" id="statusBadge">—</span>
    </div>

    <div class="detail-row">
      <div class="detail-label">Identified Disease</div>
      <div class="detail-value" id="diseaseName">—</div>
      <div class="confidence-pill" id="confidencePill">—</div>
    </div>

    <div class="detail-row">
      <div class="detail-label">Visible Symptoms</div>
      <div class="detail-value" id="symptomsText">—</div>
    </div>

    <div class="cure-box">
      <div class="detail-label">🌱 Organic Treatment</div>
      <div class="detail-value" id="organicCureText">—</div>
    </div>
    <div class="cure-box" style="margin-top: 12px;">
      <div class="detail-label">🧪 Chemical Treatment</div>
      <div class="detail-value" id="chemicalCureText">—</div>
    </div>
  </div>
</div>

<script>
  (function() {
    'use strict';

    var apiKeyInput = document.getElementById('apiKeyInput');
    var toggleApiKeyBtn = document.getElementById('toggleApiKeyBtn');
    var saveKeyBtn = document.getElementById('saveKeyBtn');
    var cameraVideo = document.getElementById('cameraVideo');
    var cameraPlaceholder = document.getElementById('cameraPlaceholder');
    var captureBtn = document.getElementById('captureBtn');
    var fileInput = document.getElementById('fileInput');
    var previewSection = document.getElementById('previewSection');
    var previewCanvas = document.getElementById('previewCanvas');
    var loadingContainer = document.getElementById('loadingContainer');
    var statusText = document.getElementById('statusText');
    var errorBanner = document.getElementById('errorBanner');
    var errorMessage = document.getElementById('errorMessage');
    var resultCard = document.getElementById('resultCard');
    var plantNameEl = document.getElementById('plantName');
    var statusBadge = document.getElementById('statusBadge');
    var diseaseNameEl = document.getElementById('diseaseName');
    var confidencePill = document.getElementById('confidencePill');
    var symptomsTextEl = document.getElementById('symptomsText');
    var organicCureTextEl = document.getElementById('organicCureText');
    var chemicalCureTextEl = document.getElementById('chemicalCureText');

    var videoStream = null;
    var isProcessing = false;

    function loadApiKey() {
      try {
        var saved = localStorage.getItem('gemini_api_key');
        if (saved) {
          apiKeyInput.value = saved;
        }
      } catch (e) {
        console.warn('Could not load API key from localStorage:', e);
      }
    }

    function saveApiKey() {
      var key = apiKeyInput.value.trim();
      try {
        if (key) {
          localStorage.setItem('gemini_api_key', key);
          showTemporaryMessage('API key saved');
        } else {
          localStorage.removeItem('gemini_api_key');
          showTemporaryMessage('API key cleared');
        }
      } catch (e) {
        console.warn('Could not save API key:', e);
      }
    }

    function showTemporaryMessage(msg) {
      if (!loadingContainer.classList.contains('hidden')) return;
      errorBanner.classList.remove('hidden');
      errorBanner.style.background = '#d4edda';
      errorBanner.style.borderLeftColor = '#2e7d32';
      errorMessage.textContent = msg;
      errorMessage.style.color = '#0b5e0b';
      setTimeout(function() {
        errorBanner.classList.add('hidden');
        errorBanner.style.background = '';
        errorBanner.style.borderLeftColor = '#c62828';
        errorMessage.style.color = '';
      }, 1800);
    }

    toggleApiKeyBtn.addEventListener('click', function() {
      var type = apiKeyInput.type === 'password' ? 'text' : 'password';
      apiKeyInput.type = type;
      toggleApiKeyBtn.textContent = type === 'password' ? '👁️' : '🙈';
    });

    saveKeyBtn.addEventListener('click', saveApiKey);

    function startCamera() {
      if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
        cameraVideo.style.display = 'none';
        cameraPlaceholder.style.display = 'flex';
        cameraPlaceholder.innerHTML = '<span>📷</span><p>Camera not supported. Use upload instead.</p>';
        captureBtn.disabled = true;
        return;
      }

      if (videoStream) {
        videoStream.getTracks().forEach(function(track) { track.stop(); });
        videoStream = null;
      }

      navigator.mediaDevices.getUserMedia({
        video: { facingMode: 'environment', width: { ideal: 640 }, height: { ideal: 640 } },
        audio: false
      }).then(function(stream) {
        videoStream = stream;
        cameraVideo.srcObject = stream;
        cameraVideo.style.display = 'block';
        cameraPlaceholder.style.display = 'none';
        captureBtn.disabled = false;
      }).catch(function(err) {
        console.warn('Camera error:', err);
        cameraVideo.style.display = 'none';
        cameraPlaceholder.style.display = 'flex';
        cameraPlaceholder.innerHTML = '<span>📷</span><p>Camera not available. Use upload instead.</p>';
        captureBtn.disabled = true;
      });
    }

    window.addEventListener('load', function() {
      loadApiKey();
      startCamera();
    });

    window.addEventListener('beforeunload', function() {
      if (videoStream) {
        videoStream.getTracks().forEach(function(track) { track.stop(); });
      }
    });

    cameraVideo.addEventListener('loadedmetadata', function() {
      cameraVideo.play().catch(function(e) { console.warn('Video play error:', e); });
    });

    function captureFromVideo() {
      if (!videoStream || !cameraVideo.videoWidth) return null;
      var video = cameraVideo;
      var canvas = document.createElement('canvas');
      var size = Math.min(video.videoWidth, video.videoHeight);
      canvas.width = size;
      canvas.height = size;
      var ctx = canvas.getContext('2d');
      var offsetX = (video.videoWidth - size) / 2;
      var offsetY = (video.videoHeight - size) / 2;
      ctx.drawImage(video, offsetX, offsetY, size, size, 0, 0, size, size);
      return canvas;
    }

    function showPreviewFromCanvas(sourceCanvas) {
      var ctx = previewCanvas.getContext('2d');
      previewCanvas.width = sourceCanvas.width;
      previewCanvas.height = sourceCanvas.height;
      ctx.drawImage(sourceCanvas, 0, 0);
      previewSection.classList.remove('hidden');
    }

    function showPreviewFromImage(img) {
      var canvas = document.createElement('canvas');
      var maxDim = 640;
      var width = img.width;
      var height = img.height;
      if (width > height && width > maxDim) {
        height = (height * maxDim) / width;
        width = maxDim;
      } else if (height > maxDim) {
        width = (width * maxDim) / height;
        height = maxDim;
      }
      canvas.width = width;
      canvas.height = height;
      var ctx = canvas.getContext('2d');
      ctx.drawImage(img, 0, 0, width, height);
      showPreviewFromCanvas(canvas);
      return canvas;
    }

    function canvasToBase64Jpeg(canvas) {
      var dataUrl = canvas.toDataURL('image/jpeg', 0.85);
      return dataUrl.split(',')[1];
    }

    function setLoading(active, message) {
      if (active) {
        loadingContainer.classList.remove('hidden');
        statusText.textContent = message || 'Scanning leaf structure...';
        captureBtn.disabled = true;
      } else {
        loadingContainer.classList.add('hidden');
        captureBtn.disabled = false;
      }
    }

    function showError(msg) {
      errorBanner.classList.remove('hidden');
      errorMessage.textContent = msg;
      errorBanner.style.background = '#ffebee';
      errorBanner.style.borderLeftColor = '#c62828';
      errorMessage.style.color = '#b71c1c';
    }

    function clearError() {
      errorBanner.classList.add('hidden');
    }

    function hideResult() {
      resultCard.classList.add('hidden');
    }

    function extractJsonFromResponse(text) {
      var cleaned = text.trim();
      var codeBlockMatch = cleaned.match(/```(?:json)?\s*([\s\S]*?)```/i);
      if (codeBlockMatch) {
        cleaned = codeBlockMatch[1].trim();
      }
      var firstBrace = cleaned.indexOf('{');
      var lastBrace = cleaned.lastIndexOf('}');
      if (firstBrace !== -1 && lastBrace !== -1 && lastBrace > firstBrace) {
        cleaned = cleaned.slice(firstBrace, lastBrace + 1);
      }
      try {
        return JSON.parse(cleaned);
      } catch (e) {
        console.warn('JSON parse error:', e);
        return null;
      }
    }

    function analyzeImage(canvas) {
      clearError();
      hideResult();

      var apiKey = apiKeyInput.value.trim();
      if (!apiKey) {
        showError('Please enter your Google Gemini API key first.');
        return Promise.resolve();
      }

      if (!canvas) {
        showError('No image captured. Please try again.');
        return Promise.resolve();
      }

      var base64Data = canvasToBase64Jpeg(canvas);
      if (!base64Data || base64Data.length < 100) {
        showError('Failed to process image. Please try again.');
        return Promise.resolve();
      }

      setLoading(true, 'Scanning leaf structure...');

      var statusMessages = [
        'Scanning leaf structure...',
        'Consulting AI model...',
        'Diagnosing plant health...',
        'Generating recommendations...'
      ];
      var statusIndex = 0;
      var statusInterval = setInterval(function() {
        if (statusIndex < statusMessages.length - 1) {
          statusIndex++;
          statusText.textContent = statusMessages[statusIndex];
        }
      }, 1200);

      var url = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-3.8-flash:generateContent?key=' + encodeURIComponent(apiKey);

      var payload = {
        contents: [{
          parts: [
            {
              text: "You are an agricultural plant pathologist. Analyze this leaf image. Respond in clean JSON with these keys: plant_name, health_status ('Healthy' or 'Diseased'), disease_name, confidence_level, symptoms, organic_cure, chemical_cure. Keep explanations concise and actionable."
            },
            {
              inlineData: {
                mimeType: "image/jpeg",
                data: base64Data
              }
            }
          ]
        }]
      };

      return fetch(url, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      }).then(function(response) {
        clearInterval(statusInterval);

        if (!response.ok) {
          return response.text().then(function(errText) {
            var errorDetail = '';
            try {
              var errJson = JSON.parse(errText);
              errorDetail = (errJson && errJson.error && errJson.error.message) ? errJson.error.message : errText;
            } catch (e) {
              errorDetail = errText || response.statusText;
            }
            throw new Error('API error (' + response.status + '): ' + (errorDetail || 'Request failed'));
          });
        }

        return response.json();
      }).then(function(data) {
        var responseText = '';
        if (data && data.candidates && data.candidates[0] && data.candidates[0].content && data.candidates[0].content.parts) {
          var parts = data.candidates[0].content.parts;
          for (var i = 0; i < parts.length; i++) {
            if (parts[i].text) {
              responseText += parts[i].text;
            }
          }
        }

        if (!responseText) {
          throw new Error('No valid response from AI model.');
        }

        var parsed = extractJsonFromResponse(responseText);
        if (!parsed) {
          throw new Error('Could not parse diagnosis. Please try again.');
        }

        displayDiagnosis(parsed);
      }).catch(function(error) {
        console.error('Analysis error:', error);
        showError(error.message || 'Failed to analyze image. Check your API key and connection.');
      }).then(function() {
        clearInterval(statusInterval);
        setLoading(false);
      });
    }

    function displayDiagnosis(data) {
      var plantName = data.plant_name || 'Unknown plant';
      var healthStatus = data.health_status || 'Unknown';
      var diseaseName = data.disease_name || 'None detected';
      var confidence = data.confidence_level || '';
      var symptoms = data.symptoms || 'No symptoms described.';
      var organicCure = data.organic_cure || 'No organic treatment suggested.';
      var chemicalCure = data.chemical_cure || 'No chemical treatment suggested.';

      plantNameEl.textContent = plantName;

      var isHealthy = healthStatus.toLowerCase().indexOf('healthy') !== -1;
      statusBadge.textContent = healthStatus;
      statusBadge.className = 'status-badge ' + (isHealthy ? 'healthy' : 'diseased');

      diseaseNameEl.textContent = diseaseName;

      if (confidence) {
        confidencePill.textContent = 'Confidence: ' + confidence;
        confidencePill.classList.remove('hidden');
      } else {
        confidencePill.textContent = '';
        confidencePill.classList.add('hidden');
      }

      symptomsTextEl.textContent = symptoms;
      organicCureTextEl.textContent = organicCure;
      chemicalCureTextEl.textContent = chemicalCure;

      resultCard.classList.remove('hidden');
    }

    captureBtn.addEventListener('click', function() {
      if (isProcessing) return;
      isProcessing = true;
      var canvas = captureFromVideo();
      if (!canvas) {
        showError('Camera not ready. Please allow camera access or upload a photo.');
        isProcessing = false;
        return;
      }
      showPreviewFromCanvas(canvas);
      analyzeImage(canvas).then(function() {
        isProcessing = false;
      });
    });

    fileInput.addEventListener('change', function(e) {
      var file = e.target.files[0];
      if (!file) return;

      if (!file.type.startsWith('image/')) {
        showError('Please select an image file.');
        return;
      }

      var reader = new FileReader();
      reader.onload = function(event) {
        var img = new Image();
        img.onload = function() {
          var canvas = showPreviewFromImage(img);
          fileInput.value = '';
          analyzeImage(canvas);
        };
        img.onerror = function() {
          showError('Failed to load image. Try another file.');
        };
        img.src = event.target.result;
      };
      reader.readAsDataURL(file);
    });

  })();
</script>
</body>
</html>
