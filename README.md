<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>XzeCure - Patient Visit Form</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css">
    <style>
        :root {
            --primary-color: #2c3e50;
            --secondary-color: #34495e;
            --accent-blue: #007bff;
            --accent-green: #2ecc71;
            --text-color: #ecf0f1;
        }

        body {
            background-color: var(--primary-color);
            color: var(--text-color);
        }

        .container {
            max-width: 800px;
        }

        .card {
            background-color: var(--secondary-color);
            border: none;
            color: var(--text-color);
            margin-bottom: 20px;
        }

        .card-header {
            background-color: var(--primary-color);
            border-bottom: 1px solid var(--secondary-color);
            padding: 1rem;
        }

        .form-control, .form-select, .btn {
            background-color: #495d73;
            color: var(--text-color);
            border: 1px solid #5d758c;
        }

        .form-control:focus, .form-select:focus {
            background-color: #495d73;
            color: var(--text-color);
            box-shadow: 0 0 0 0.25rem rgba(0, 123, 255, 0.25);
        }

        .btn-primary {
            background-color: var(--accent-blue);
            border-color: var(--accent-blue);
        }

        .btn-primary:hover {
            background-color: #0056b3;
            border-color: #004c99;
        }

        .btn-success {
            background-color: var(--accent-green);
            border-color: var(--accent-green);
        }
        
        .btn-success:hover {
            background-color: #27ae60;
            border-color: #229954;
        }

        .form-check-input:checked {
            background-color: var(--accent-blue);
            border-color: var(--accent-blue);
        }

        .badge {
            font-size: 0.8em;
            vertical-align: top;
            margin-left: 5px;
        }

        .image-preview img {
            max-width: 100px;
            max-height: 100px;
            margin: 5px;
            border: 2px solid var(--accent-blue);
            border-radius: 5px;
        }

        .form-label i {
            margin-right: 5px;
        }

        #pdf-preview-container {
            position: relative;
            width: 100%;
            height: 0;
            padding-top: 141.42%; /* A4 aspect ratio: 210mm / 297mm */
            background-color: #fff;
            overflow: hidden;
            display: none;
        }

        #pdf-preview-container iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .spinner-border {
            width: 3rem;
            height: 3rem;
        }
        
        #letterhead-preview {
            max-width: 100%;
            height: auto;
            border: 2px dashed #ccc;
            margin-top: 10px;
        }

        @media (min-width: 768px) {
            .two-column-form .row > div {
                flex: 0 0 50%;
                max-width: 50%;
            }
        }
    </style>
</head>
<body>

    <div class="loading-overlay" id="loadingOverlay">
        <div class="spinner-border text-light" role="status">
            <span class="visually-hidden">Loading...</span>
        </div>
    </div>

    <div class="container py-4">
        <div class="d-flex align-items-center mb-4">
            <img src="https://i.imgur.com/Kz9Y6nB.png" alt="XzeCure Logo" width="50" class="me-3">
            <h1 class="h3 mb-0">XzeCure Patient Visit Form</h1>
        </div>

        <div class="card">
            <div class="card-body">
                <p class="mb-1 text-muted"><i class="fas fa-info-circle"></i> This app stores data locally on this device only. If you need centralized storage, request a different version.</p>
                <hr>
                <div class="d-flex justify-content-between align-items-center mb-3">
                    <h5 class="card-title mb-0">Create New Visit</h5>
                    <button class="btn btn-sm btn-outline-info" type="button" data-bs-toggle="collapse" data-bs-target="#savedVisitsList" aria-expanded="false" aria-controls="savedVisitsList">
                        <i class="fas fa-history"></i> Saved Visits
                    </button>
                </div>
                <div class="collapse" id="savedVisitsList">
                    <ul id="savedVisits" class="list-group list-group-flush mb-3"></ul>
                </div>

                <form id="visitForm" class="two-column-form">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="staffName" class="form-label"><i class="fas fa-user-md"></i> Staff/Doctor Name <span class="badge bg-danger">Required</span></label>
                            <input type="text" class="form-control" id="staffName" required placeholder="Dr. Jane Doe">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="patientName" class="form-label"><i class="fas fa-user-circle"></i> Patient Name <span class="badge bg-danger">Required</span></label>
                            <input type="text" class="form-control" id="patientName" required placeholder="John Smith">
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="age" class="form-label"><i class="fas fa-birthday-cake"></i> Age</label>
                            <input type="number" class="form-control" id="age" placeholder="35">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="contactNumber" class="form-label"><i class="fas fa-phone-alt"></i> Contact Number</label>
                            <input type="tel" class="form-control" id="contactNumber" placeholder="+91 9876543210">
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="address" class="form-label"><i class="fas fa-map-marked-alt"></i> Address</label>
                        <textarea class="form-control" id="address" rows="2" placeholder="Patient's full address"></textarea>
                    </div>
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="complaints" class="form-label"><i class="fas fa-notes-medical"></i> Current Complaints <span class="badge bg-danger">Required</span></label>
                            <textarea class="form-control" id="complaints" rows="3" required placeholder="Cough, fever, sore throat..."></textarea>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="duration" class="form-label"><i class="fas fa-clock"></i> Duration of Complaints</label>
                            <input type="text" class="form-control" id="duration" placeholder="3 days">
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="history" class="form-label"><i class="fas fa-history"></i> Previous Medical/Surgical History</label>
                        <textarea class="form-control" id="history" rows="2" placeholder="e.g., Diabetes, Hypertension, Appendectomy"></textarea>
                    </div>
                    <div class="mb-3">
                        <label for="vitals" class="form-label"><i class="fas fa-heartbeat"></i> Current Vitals</label>
                        <input type="text" class="form-control" id="vitals" placeholder="BP: 120/80 mmHg, Pulse: 85 bpm, SpO2: 98%, Temp: 98.6°F">
                    </div>
                    <div class="mb-3">
                        <label for="signs" class="form-label"><i class="fas fa-thermometer-half"></i> Specific Signs or Symptoms</label>
                        <textarea class="form-control" id="signs" rows="2" placeholder="e.g., Rash, localized pain, swelling"></textarea>
                    </div>
                    <div class="mb-3">
                        <label for="treatment" class="form-label"><i class="fas fa-prescription-bottle-alt"></i> Treatment Advised <span class="badge bg-danger">Required</span></label>
                        <textarea class="form-control" id="treatment" rows="3" required placeholder="Paracetamol 500mg, rest, hydration..."></textarea>
                    </div>
                    <div class="mb-3">
                        <label for="investigations" class="form-label"><i class="fas fa-microscope"></i> Any Blood Investigations Advised</label>
                        <textarea class="form-control" id="investigations" rows="2" placeholder="e.g., CBC, LFT, KFT"></textarea>
                    </div>
                    
                    <div class="mb-3">
                        <label class="form-label"><i class="fas fa-calendar-check"></i> Follow-up required?</label>
                        <div class="form-check form-check-inline">
                            <input class="form-check-input" type="radio" name="followup" id="followupYes" value="Yes">
                            <label class="form-check-label" for="followupYes">Yes</label>
                        </div>
                        <div class="form-check form-check-inline">
                            <input class="form-check-input" type="radio" name="followup" id="followupNo" value="No" checked>
                            <label class="form-check-label" for="followupNo">No</label>
                        </div>
                    </div>
                    <div class="mb-3" id="followupDateSection" style="display: none;">
                        <label for="followupDate" class="form-label"><i class="fas fa-calendar-alt"></i> Suggested Follow-up Date</label>
                        <input type="date" class="form-control" id="followupDate">
                    </div>

                    <div class="mb-3">
                        <label for="whatsappNumber" class="form-label"><i class="fab fa-whatsapp"></i> Patient WhatsApp Number</label>
                        <input type="tel" class="form-control" id="whatsappNumber" placeholder="Enter WhatsApp number">
                    </div>

                    <div class="mb-3">
                        <label for="photoUpload" class="form-label"><i class="fas fa-images"></i> Upload Photos (max 3)</label>
                        <input type="file" class="form-control" id="photoUpload" multiple accept="image/*">
                        <div id="photoPreview" class="mt-2 d-flex flex-wrap"></div>
                    </div>

                    <div class="mb-3">
                        <label for="letterheadUpload" class="form-label"><i class="fas fa-file-image"></i> Upload/Replace Letterhead Image</label>
                        <input type="file" class="form-control" id="letterheadUpload" accept="image/*">
                        <img id="letterhead-preview" src="https://i.imgur.com/8B1S8hD.png" alt="Letterhead Preview">
                    </div>

                    <div class="d-grid gap-2 d-md-flex justify-content-md-end mt-4">
                        <button type="button" class="btn btn-secondary" id="previewBtn"><i class="fas fa-search-plus"></i> Preview PDF</button>
                        <button type="submit" class="btn btn-primary" id="generatePdfBtn"><i class="fas fa-file-pdf"></i> Generate PDF</button>
                    </div>
                </form>
            </div>
        </div>

        <div class="card mt-4" id="pdfActionsCard" style="display: none;">
            <div class="card-body">
                <h5 class="card-title mb-3">PDF Generated!</h5>
                <div id="pdf-preview-container">
                    <iframe id="pdf-iframe" frameborder="0"></iframe>
                </div>
                <div class="d-grid gap-2 d-md-flex justify-content-md-end mt-3">
                    <button class="btn btn-success" id="downloadPdfBtn"><i class="fas fa-download"></i> Download PDF</button>
                    <button class="btn btn-success" id="sharePdfBtn"><i class="fab fa-whatsapp"></i> Share via WhatsApp</button>
                    <button class="btn btn-info" id="copyLinkBtn"><i class="fas fa-link"></i> Copy Link</button>
                </div>
                <div id="fallbackMessage" class="mt-3 text-center text-muted" style="display: none;">
                    <p>Web Share API is not supported. Please use the 'Copy Link' button and share manually.</p>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const form = document.getElementById('visitForm');
            const patientNameInput = document.getElementById('patientName');
            const contactNumberInput = document.getElementById('contactNumber');
            const whatsappNumberInput = document.getElementById('whatsappNumber');
            const followupYes = document.getElementById('followupYes');
            const followupDateSection = document.getElementById('followupDateSection');
            const photoUpload = document.getElementById('photoUpload');
            const photoPreview = document.getElementById('photoPreview');
            const letterheadUpload = document.getElementById('letterheadUpload');
            const letterheadPreview = document.getElementById('letterhead-preview');
            const previewBtn = document.getElementById('previewBtn');
            const generatePdfBtn = document.getElementById('generatePdfBtn');
            const pdfActionsCard = document.getElementById('pdfActionsCard');
            const downloadPdfBtn = document.getElementById('downloadPdfBtn');
            const sharePdfBtn = document.getElementById('sharePdfBtn');
            const copyLinkBtn = document.getElementById('copyLinkBtn');
            const pdfIframe = document.getElementById('pdf-iframe');
            const savedVisitsList = document.getElementById('savedVisits');
            const loadingOverlay = document.getElementById('loadingOverlay');
            
            let generatedPdfBlob = null;
            let photoDataUrls = [];
            const defaultLetterheadUrl = "https://i.imgur.com/8B1S8hD.png";
            
            // Initial setup
            loadSavedVisits();
            
            // Event Listeners
            contactNumberInput.addEventListener('input', () => {
                whatsappNumberInput.value = contactNumberInput.value;
            });

            followupYes.addEventListener('change', () => {
                followupDateSection.style.display = followupYes.checked ? 'block' : 'none';
            });

            photoUpload.addEventListener('change', async (event) => {
                photoPreview.innerHTML = '';
                photoDataUrls = [];
                const files = event.target.files;
                const maxFiles = 3;

                for (let i = 0; i < files.length && i < maxFiles; i++) {
                    const file = files[i];
                    if (file) {
                        const reader = new FileReader();
                        reader.onload = (e) => {
                            const img = document.createElement('img');
                            img.src = e.target.result;
                            img.className = 'img-thumbnail me-2 mb-2';
                            photoPreview.appendChild(img);
                            photoDataUrls.push(e.target.result);
                        };
                        reader.readAsDataURL(file);
                    }
                }
            });

            letterheadUpload.addEventListener('change', (event) => {
                const file = event.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        letterheadPreview.src = e.target.result;
                    };
                    reader.readAsDataURL(file);
                } else {
                    letterheadPreview.src = defaultLetterheadUrl;
                }
            });

            form.addEventListener('submit', async (e) => {
                e.preventDefault();
                await generateAndSavePdf();
            });

            previewBtn.addEventListener('click', async () => {
                await generateAndSavePdf(true);
            });

            downloadPdfBtn.addEventListener('click', () => {
                if (generatedPdfBlob) {
                    const patientName = patientNameInput.value.trim().replace(/\s+/g, '_');
                    const date = new Date().toISOString().slice(0, 10).replace(/-/g, '');
                    saveAs(generatedPdfBlob, `XzeCure_Visit_${patientName}_${date}.pdf`);
                }
            });

            sharePdfBtn.addEventListener('click', async () => {
                if (!generatedPdfBlob) return;

                const staffName = document.getElementById('staffName').value.trim();
                const patientName = patientNameInput.value.trim();
                const visitDate = new Date().toLocaleDateString('en-US');
                const textMessage = `XzeCure Visit Details for ${patientName} on ${visitDate} (Dr. ${staffName}).`;

                const whatsappNumber = whatsappNumberInput.value.trim();

                try {
                    if (navigator.share && navigator.canShare({ files: [new File([generatedPdfBlob], 'visit.pdf', { type: 'application/pdf' })] })) {
                        await navigator.share({
                            title: 'XzeCure Visit PDF',
                            text: textMessage,
                            files: [new File([generatedPdfBlob], 'XzeCure_Visit.pdf', { type: 'application/pdf' })],
                        });
                    } else {
                        // Fallback for browsers that don't support file sharing
                        const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(textMessage)}`;
                        window.open(whatsappUrl, '_blank');
                    }
                } catch (error) {
                    console.error('Sharing failed', error);
                    // Fallback in case of any error
                    const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(textMessage)}`;
                    window.open(whatsappUrl, '_blank');
                }
            });

            copyLinkBtn.addEventListener('click', () => {
                if (generatedPdfBlob) {
                    const url = URL.createObjectURL(generatedPdfBlob);
                    navigator.clipboard.writeText(url).then(() => {
                        alert('PDF link copied to clipboard! You can share this link with the patient (note: it will expire when you close the tab).');
                    }).catch(err => {
                        console.error('Failed to copy text: ', err);
                        alert('Failed to copy link. Please try again.');
                    });
                }
            });

            // Functions
            async function generateAndSavePdf(isPreview = false) {
                if (!form.checkValidity()) {
                    form.classList.add('was-validated');
                    return;
                }

                loadingOverlay.style.display = 'flex';
                
                const patientName = patientNameInput.value.trim();
                const staffName = document.getElementById('staffName').value.trim();
                const date = new Date();
                const formattedDate = date.toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' });
                const formattedTime = date.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' });

                // Create a temporary element for PDF content
                const pdfContentDiv = document.createElement('div');
                pdfContentDiv.style.width = '210mm';
                pdfContentDiv.style.minHeight = '297mm';
                pdfContentDiv.style.backgroundColor = '#1a2430';
                pdfContentDiv.style.color = 'white';
                pdfContentDiv.style.fontFamily = 'sans-serif';
                pdfContentDiv.style.fontSize = '12pt';
                pdfContentDiv.style.position = 'relative';
                pdfContentDiv.style.padding = '20mm';
                pdfContentDiv.style.boxSizing = 'border-box';

                // Add background image
                const letterheadImg = letterheadPreview.src;
                pdfContentDiv.style.backgroundImage = `url('${letterheadImg}')`;
                pdfContentDiv.style.backgroundSize = 'contain';
                pdfContentDiv.style.backgroundRepeat = 'no-repeat';
                pdfContentDiv.style.backgroundPosition = 'center';

                // Add content
                const contentHtml = `
                    <div style="position: relative; z-index: 10;">
                        <h1 style="text-align: right; margin-bottom: 20px; color: white; font-size: 20pt; text-transform: uppercase;">XzeCure Visit Report</h1>
                        <p style="text-align: right; margin-top: -10px; font-size: 10pt;">S/B (Dr ${staffName})</p>
                        <hr style="border-top: 1px solid rgba(255,255,255,0.5);">

                        <div style="display: flex; justify-content: space-between; margin-bottom: 10px; flex-wrap: wrap;">
                            <div style="flex: 1; min-width: 150px;">
                                <p style="margin: 0; font-size: 11pt;"><b>Patient Name:</b> ${patientName}</p>
                                <p style="margin: 0; font-size: 11pt;"><b>Age:</b> ${document.getElementById('age').value || 'N/A'}</p>
                            </div>
                            <div style="flex: 1; min-width: 150px;">
                                <p style="margin: 0; font-size: 11pt;"><b>Date:</b> ${formattedDate}</p>
                                <p style="margin: 0; font-size: 11pt;"><b>Time:</b> ${formattedTime}</p>
                            </div>
                        </div>

                        ${document.getElementById('contactNumber').value ? `<p style="margin: 0; margin-bottom: 5px; font-size: 11pt;"><b>Contact:</b> ${document.getElementById('contactNumber').value}</p>` : ''}
                        ${document.getElementById('address').value ? `<p style="margin: 0; margin-bottom: 5px; font-size: 11pt;"><b>Address:</b> ${document.getElementById('address').value}</p>` : ''}
                        
                        <div style="margin-top: 20px;">
                            <h4 style="color: white; font-size: 14pt;">Visit Details</h4>
                            <ul style="list-style-type: none; padding: 0;">
                                <li><b>Current Complaints:</b> ${document.getElementById('complaints').value}</li>
                                ${document.getElementById('duration').value ? `<li><b>Duration of Complaints:</b> ${document.getElementById('duration').value}</li>` : ''}
                                ${document.getElementById('history').value ? `<li><b>Previous History:</b> ${document.getElementById('history').value}</li>` : ''}
                                ${document.getElementById('vitals').value ? `<li><b>Current Vitals:</b> ${document.getElementById('vitals').value}</li>` : ''}
                                ${document.getElementById('signs').value ? `<li><b>Signs/Symptoms:</b> ${document.getElementById('signs').value}</li>` : ''}
                                <li><b>Treatment Advised:</b> ${document.getElementById('treatment').value}</li>
                                ${document.getElementById('investigations').value ? `<li><b>Investigations:</b> ${document.getElementById('investigations').value}</li>` : ''}
                            </ul>
                        </div>

                        <div style="margin-top: 20px;">
                            <h4 style="color: white; font-size: 14pt;">Follow-up</h4>
                            <p style="margin: 0;"><b>Follow-up required?</b> ${document.querySelector('input[name="followup"]:checked').value}</p>
                            ${document.getElementById('followupDate').value ? `<p style="margin: 0;"><b>Suggested Date:</b> ${document.getElementById('followupDate').value}</p>` : ''}
                        </div>
                        
                        ${photoDataUrls.length > 0 ? `
                        <div style="margin-top: 20px;">
                            <h4 style="color: white; font-size: 14pt;">Attached Photos</h4>
                            <div style="display: flex; flex-wrap: wrap; gap: 10px;">
                                ${photoDataUrls.map(url => `<img src="${url}" style="width: 100px; height: auto; border: 2px solid #ccc; border-radius: 5px;">`).join('')}
                            </div>
                        </div>
                        ` : ''}
                    </div>
                    <div style="position: absolute; bottom: 10mm; left: 20mm; right: 20mm; text-align: right; font-size: 10pt; color: white;">
                        <p style="margin: 0;">Generated by XzeCure on ${date.toLocaleString()}</p>
                        <p style="margin: 0;">Signed by: Dr. ${staffName}</p>
                    </div>
                `;

                pdfContentDiv.innerHTML = contentHtml;
                document.body.appendChild(pdfContentDiv);

                try {
                    const canvas = await html2canvas(pdfContentDiv, { scale: 2, useCORS: true });
                    const imgData = canvas.toDataURL('image/jpeg', 1.0);
                    const { jsPDF } = window.jspdf;
                    const pdf = new jsPDF('p', 'mm', 'a4');
                    const imgProps= pdf.getImageProperties(imgData);
                    const pdfWidth = pdf.internal.pageSize.getWidth();
                    const pdfHeight = (imgProps.height * pdfWidth) / imgProps.width;
                    pdf.addImage(imgData, 'JPEG', 0, 0, pdfWidth, pdfHeight);

                    const pdfBlob = pdf.output('blob');
                    generatedPdfBlob = pdfBlob;

                    if (isPreview) {
                        const pdfUrl = URL.createObjectURL(pdfBlob);
                        pdfIframe.src = pdfUrl;
                        pdfActionsCard.style.display = 'block';
                        document.getElementById('pdf-preview-container').style.display = 'block';
                        window.scrollTo({
                            top: pdfActionsCard.offsetTop,
                            behavior: 'smooth'
                        });
                    } else {
                        // Save to local storage
                        const patientInfo = {
                            name: patientName,
                            date: date.toISOString(),
                            staff: staffName
                        };
                        const visits = JSON.parse(localStorage.getItem('xzecure_visits') || '[]');
                        visits.push(patientInfo);
                        localStorage.setItem('xzecure_visits', JSON.stringify(visits));
                        loadSavedVisits();
                        alert('PDF generated and saved locally! You can now download or share it.');
                        pdfActionsCard.style.display = 'block';
                        document.getElementById('pdf-preview-container').style.display = 'none';
                    }

                } catch (error) {
                    console.error('Error generating PDF:', error);
                    alert('An error occurred while generating the PDF. Please try again.');
                } finally {
                    document.body.removeChild(pdfContentDiv);
                    loadingOverlay.style.display = 'none';
                }
            }

            function loadSavedVisits() {
                const visits = JSON.parse(localStorage.getItem('xzecure_visits') || '[]');
                savedVisitsList.innerHTML = '';
                if (visits.length === 0) {
                    savedVisitsList.innerHTML = '<li class="list-group-item">No saved visits found.</li>';
                    return;
                }

                visits.forEach((visit, index) => {
                    const li = document.createElement('li');
                    li.className = 'list-group-item d-flex justify-content-between align-items-center';
                    const visitDate = new Date(visit.date).toLocaleDateString();
                    li.innerHTML = `
                        <div>
                            <strong>${visit.name}</strong><br>
                            <small class="text-muted">Staff: ${visit.staff} | Date: ${visitDate}</small>
                        </div>
                    `;
                    savedVisitsList.appendChild(li);
                });
            }
        });
    </script>
</body>
</html>