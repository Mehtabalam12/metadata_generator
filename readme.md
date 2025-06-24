# 🚀 Streamlit Metadata Generator Deployment Guide

## 📁 Project Structure

Create the following files in your project directory:

```
metadata-generator/
├── app.py                 # Main Streamlit application
├── requirements.txt       # Python dependencies
├── packages.txt          # System dependencies (for Streamlit Cloud)
├── setup.sh              # Setup script
└── README.md             # Documentation
```

## 📝 File Contents

### 1. requirements.txt
```
streamlit==1.29.0
PyPDF2==3.0.1
python-docx==0.8.11
openpyxl==3.1.2
pytesseract==0.3.10
Pillow==10.1.0
nltk==3.8.1
spacy==3.7.2
textstat==0.7.3
langdetect==1.0.9
```

### 2. packages.txt (for Streamlit Cloud)
```
tesseract-ocr
tesseract-ocr-eng
```

### 3. setup.sh (for local setup)
```bash
#!/bin/bash
echo "Setting up Metadata Generator..."

# Install system dependencies (Ubuntu/Debian)
sudo apt-get update
sudo apt-get install -y tesseract-ocr tesseract-ocr-eng

# Install Python packages
pip install -r requirements.txt

# Download spaCy model
python -m spacy download en_core_web_sm

echo "Setup complete!"
```

## 🖥️ Local Development Setup

### Step 1: Clone/Create Project
```bash
mkdir metadata-generator
cd metadata-generator
```

### Step 2: Create Virtual Environment
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
# Install Python packages
pip install -r requirements.txt

# Install system dependencies (Ubuntu/Debian)
sudo apt-get update
sudo apt-get install tesseract-ocr tesseract-ocr-eng

# Download spaCy model
python -m spacy download en_core_web_sm
```

### Step 4: Run Locally
```bash
streamlit run app.py
```

The app will open in your browser at `http://localhost:8501`

## ☁️ Streamlit Cloud Deployment

### Step 1: Prepare Repository
1. Create a GitHub repository
2. Upload all project files:
   - `app.py`
   - `requirements.txt`
   - `packages.txt`
   - `README.md`

### Step 2: Deploy to Streamlit Cloud
1. Go to [share.streamlit.io](https://share.streamlit.io)
2. Sign in with GitHub
3. Click "New app"
4. Select your repository
5. Set main file path: `app.py`
6. Click "Deploy"

### Step 3: Configure Advanced Settings (if needed)
- **Python version**: 3.9+
- **Environment variables**: None required
- **Secrets**: None required for basic functionality

## 🐳 Docker Deployment

### Dockerfile
```dockerfile
FROM python:3.9-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    tesseract-ocr \
    tesseract-ocr-eng \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python packages
COPY requirements.txt .
RUN pip install -r requirements.txt

# Download spaCy model
RUN python -m spacy download en_core_web_sm

# Copy application code
COPY app.py .

# Expose port
EXPOSE 8501

# Health check
HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

# Run the application
ENTRYPOINT ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

### Build and Run Docker Container
```bash
# Build the image
docker build -t metadata-generator .

# Run the container
docker run -p 8501:8501 metadata-generator
```

## 🔧 Configuration Options

### Environment Variables
You can set these in your deployment environment:

```bash
# Maximum file upload size (in MB)
STREAMLIT_SERVER_MAX_UPLOAD_SIZE=200

# Server configuration
STREAMLIT_SERVER_PORT=8501
STREAMLIT_SERVER_ADDRESS=0.0.0.0
```

### Streamlit Configuration (config.toml)
Create `.streamlit/config.toml`:

```toml
[server]
port = 8501
maxUploadSize = 200

[theme]
primaryColor = "#FF6B6B"
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F0F2F6"
textColor = "#262730"
font = "sans serif"
```

## 🚀 Production Deployment Options

### 1. Heroku Deployment
```bash
# Install Heroku CLI
# Create Procfile
echo "web: streamlit run app.py --server.port=\$PORT --server.address=0.0.0.0" > Procfile

# Create runtime.txt
echo "python-3.9.18" > runtime.txt

# Deploy
heroku create your-app-name
git push heroku main
```

### 2. AWS EC2 Deployment
```bash
# Connect to EC2 instance
ssh -i your-key.pem ubuntu@your-ec2-ip

# Install dependencies
sudo apt update
sudo apt install python3-pip nginx
pip3 install -r requirements.txt

# Run with systemd (production)
sudo systemctl enable your-app.service
sudo systemctl start your-app.service
```

### 3. Google Cloud Platform
```bash
# Install gcloud CLI
gcloud app deploy app.yaml
```

## 🔍 Troubleshooting

### Common Issues

**1. Tesseract not found**
```bash
# Ubuntu/Debian
sudo apt-get install tesseract-ocr

# macOS
brew install tesseract

# Windows
# Download from: https://github.com/UB-Mannheim/tesseract/wiki
```

**2. spaCy model missing**
```bash
python -m spacy download en_core_web_sm
```

**3. Memory issues**
- Increase server memory allocation
- Use lighter spaCy models
- Process files in chunks

**4. Streamlit Cloud specific**
- Check `packages.txt` for system dependencies
- Verify `requirements.txt` versions
- Check deployment logs

### Performance Optimization

**For Large Files:**
```python
# Add to app.py
st.set_page_config(
    page_title="Metadata Generator",
    layout="wide",
    initial_sidebar_state="expanded"
)

# Add caching for models
@st.cache_resource
def load_spacy_model():
    return spacy.load("en_core_web_sm")
```

## 📊 Monitoring and Analytics

### Add Analytics (Optional)
```python
# Add to app.py
import streamlit.components.v1 as components

# Google Analytics
components.html("""
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
""", height=0)
```

## 🛡️ Security Considerations

1. **File Upload Limits**: Set appropriate file size limits
2. **Input Validation**: Validate file types and content
3. **Resource Limits**: Monitor CPU and memory usage
4. **HTTPS**: Use HTTPS in production
5. **Rate Limiting**: Implement rate limiting for API calls

## 📈 Scaling Options

### Horizontal Scaling
- Use load balancers
- Deploy multiple instances
- Use container orchestration (Kubernetes)

### Vertical Scaling
- Increase server resources
- Optimize code performance
- Use caching strategies

## 🎯 Next Steps

1. **Deploy to Streamlit Cloud** for quick testing
2. **Set up monitoring** and error tracking
3. **Add user authentication** if needed
4. **Implement caching** for better performance
5. **Add more file formats** support
6. **Create API endpoints** for programmatic access

## 📞 Support

- **Documentation**: Check inline code comments
- **Issues**: Create GitHub issues for bugs
- **Performance**: Monitor resource usage
- **Updates**: Keep dependencies
