# 🤖 Automated Metadata Generation System

A comprehensive Python-based system that automatically extracts and generates detailed metadata from various document formats using machine learning and natural language processing techniques.

## 🌟 Features

### Document Support
- **PDF Documents** - Full text extraction from all pages
- **Microsoft Word** (.docx) - Complete document content analysis
- **Excel Spreadsheets** (.xlsx, .xls) - Multi-sheet data extraction
- **Text Files** (.txt) - Plain text analysis
- **Images** (.png, .jpg, .jpeg) - OCR-powered text extraction

### Metadata Analysis
- **📊 Content Statistics** - Word count, character count, paragraph analysis
- **🔤 Language Detection** - Automatic language identification
- **📖 Readability Scores** - Flesch-Kincaid, Coleman-Liau, and more
- **🔍 Keyword Extraction** - Most frequent and important terms
- **🏷️ Named Entity Recognition** - People, organizations, locations, dates
- **📋 Document Structure** - Headers, bullet points, numbering detection
- **⏱️ Reading Time** - Estimated time to read the document
- **🔐 File Information** - Size, creation date, hash, MIME type

## 🚀 Quick Start

### Option 1: Google Colab (Recommended)
1. Open [Google Colab](https://colab.research.google.com/)
2. Create a new notebook
3. Copy and paste the entire code from `paste.txt`
4. Run all cells
5. Use the generated public URL to access the web interface

### Option 2: Local Installation

#### Prerequisites
- Python 3.7 or higher
- pip package manager
- Tesseract OCR (for image text extraction)

#### Installation Steps

1. **Clone or Download** the project files

2. **Install System Dependencies** (Ubuntu/Debian):
   ```bash
   sudo apt-get update
   sudo apt-get install tesseract-ocr
   ```

   **macOS** (using Homebrew):
   ```bash
   brew install tesseract
   ```

   **Windows**:
   - Download and install Tesseract from [GitHub releases](https://github.com/UB-Mannheim/tesseract/wiki)

3. **Install Python Packages**:
   ```bash
   pip install gradio PyPDF2 python-docx openpyxl pytesseract Pillow transformers torch nltk spacy textstat langdetect
   ```

4. **Download Language Models**:
   ```bash
   python -m spacy download en_core_web_sm
   ```

5. **Run the System**:
   ```bash
   python metadata_generator.py
   ```

## 💻 Usage

### Web Interface
1. **Upload Document**: Click "📁 Upload Document" and select your file
2. **Generate Metadata**: Click "🔍 Generate Metadata" button
3. **View Results**: See the analysis summary and detailed metadata
4. **Download**: Save the complete metadata as a JSON file

### Supported File Types
| Format | Extension | Features |
|--------|-----------|----------|
| PDF | .pdf | Multi-page text extraction |
| Word | .docx | Full document content |
| Excel | .xlsx, .xls | Multi-sheet analysis |
| Text | .txt | UTF-8 encoding support |
| Images | .png, .jpg, .jpeg | OCR text extraction |

## 📋 Metadata Output

The system generates comprehensive metadata in JSON format:

```json
{
  "generation_timestamp": "2025-01-15T10:30:00",
  "file_information": {
    "filename": "document.pdf",
    "file_size_mb": 2.5,
    "mime_type": "application/pdf",
    "file_hash": "abc123...",
    "created_date": "2025-01-15T09:00:00"
  },
  "content_statistics": {
    "word_count": 1500,
    "character_count": 8500,
    "sentence_count": 75,
    "paragraph_count": 12
  },
  "language": "en",
  "readability_scores": {
    "flesch_reading_ease": 65.2,
    "flesch_kincaid_grade": 8.5,
    "reading_time_minutes": 6.2
  },
  "keywords": [
    {"word": "analysis", "frequency": 15},
    {"word": "data", "frequency": 12}
  ],
  "named_entities": [
    {"text": "John Smith", "label": "PERSON"},
    {"text": "New York", "label": "GPE"}
  ]
}
```

## 🔧 Configuration

### OCR Settings
For better OCR results with images, you can modify the `pytesseract` configuration:

```python
# Add to the extract_text_from_image function
custom_config = r'--oem 3 --psm 6'
text = pytesseract.image_to_string(image, config=custom_config)
```

### Processing Limits
- Text processing is limited to 1M characters for performance
- Entity extraction shows top 20 entities
- Keywords limited to top 10 by default

## 🛠️ Troubleshooting

### Common Issues

**1. Tesseract OCR not found**
```bash
# Ubuntu/Debian
sudo apt-get install tesseract-ocr

# Verify installation
tesseract --version
```

**2. spaCy model missing**
```bash
python -m spacy download en_core_web_sm
```

**3. NLTK data missing**
```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
```

**4. Memory issues with large files**
- Files over 50MB may cause memory issues
- Consider processing large files in chunks
- Increase system RAM or use cloud computing

## 📊 Performance Notes

- **PDF Processing**: ~1-2 seconds per page
- **Image OCR**: ~3-5 seconds per image
- **Text Analysis**: ~1 second per 10,000 words
- **Memory Usage**: ~500MB base + file size

## 🔒 Privacy & Security

- All processing happens locally or in your chosen environment
- No data is sent to external services (except for Gradio sharing if enabled)
- Files are processed in temporary memory and not permanently stored
- Generated URLs through Gradio sharing are temporary

## 🧪 Technical Details

### Core Libraries
- **Gradio**: Web interface framework
- **spaCy**: Natural language processing
- **NLTK**: Text analysis and tokenization
- **PyPDF2**: PDF text extraction
- **python-docx**: Word document processing
- **openpyxl**: Excel file handling
- **pytesseract**: OCR for images
- **textstat**: Readability analysis

### Architecture
```
File Upload → Text Extraction → Content Analysis → Metadata Generation → JSON Output
     ↓              ↓                ↓                    ↓              ↓
  Gradio UI    Format-specific   NLP Processing    JSON Compilation   Download
              extractors        (spaCy, NLTK)
```

## 🤝 Contributing

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Development Setup
```bash
git clone <repository-url>
cd automated-metadata-generation
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **spaCy** team for excellent NLP tools
- **Gradio** for the fantastic web interface framework
- **Tesseract** OCR engine developers
- **NLTK** contributors for text processing tools

## 📞 Support

- **Issues**: Report bugs and request features via GitHub Issues
- **Documentation**: Check the inline code comments for detailed explanations
- **Performance**: For large-scale processing, consider using cloud computing resources

---

**Made with ❤️ for automated document analysis**

*Last updated: January 2025*