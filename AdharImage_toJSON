const { createCanvas, loadImage } = require('canvas');
const { createWorker } = require('tesseract.js');

async function extractAadhaarDetails(imagePath) {
  const canvas = createCanvas(800, 600);
  const ctx = canvas.getContext('2d');

  const image = await loadImage(imagePath);
  ctx.drawImage(image, 0, 0, canvas.width, canvas.height);

  const worker = createWorker();

  await worker.load();
  await worker.loadLanguage('eng');
  await worker.initialize('eng');
  const { data: { text } } = await worker.recognize(canvas.toBuffer());
  await worker.terminate();

  const aadhaarDetails = {
    name: '',
    aadhaarNumber: '',
    dob: '',
    gender: '',
    address: '',
  };

  const nameRegExp = /^Name[:\s]+([\w\s]+)$/m;
  const aadhaarNumberRegExp = /^Aadhaar[:\s]+(\d{4}\s\d{4}\s\d{4})$/m;
  const dobRegExp = /^DOB[:\s]+([\d/]+)$/m;
  const genderRegExp = /^Gender[:\s]+(\w+)$/m;
  const addressRegExp = /^Address[:\s]+([\w\s,.]+)$/m;

  const nameMatch = text.match(nameRegExp);
  if (nameMatch) {
    aadhaarDetails.name = nameMatch[1];
  }

  const aadhaarNumberMatch = text.match(aadhaarNumberRegExp);
  if (aadhaarNumberMatch) {
    aadhaarDetails.aadhaarNumber = aadhaarNumberMatch
