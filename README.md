# Synthetic Data Generator

A comprehensive toolkit for generating high-quality synthetic datasets for AI projects, fine-tuning, and testing purposes. This project leverages the [Meta Llama Synthetic Data Kit](https://github.com/meta-llama/synthetic-data-kit) to provide a streamlined workflow for creating synthetic data from various input sources.

## 🎯 Overview

This project demonstrates how to effectively use the Meta Llama Synthetic Data Kit to generate synthetic datasets for:
- **AI Model Fine-tuning**: Create training data for custom language models
- **Testing & Validation**: Generate test datasets for AI applications
- **Research & Development**: Produce synthetic data for experimental purposes
- **Data Augmentation**: Expand existing datasets with synthetic examples

## ✨ Features

- **Multi-format Input Support**: Process PDFs, HTML files, YouTube videos, DOCX documents, PowerPoint presentations, and plain text files
- **Flexible Output Formats**: Generate QA pairs, Chain-of-Thought reasoning, and summary datasets
- **Quality Curation**: Built-in quality filtering using LLM-based evaluation
- **Multiple Export Formats**: Support for Alpaca, OpenAI fine-tuning, ChatML, and JSONL formats
- **Organized Workflow**: Structured data processing pipeline with clear separation of concerns

## 🏗️ Project Structure

```
syntetic-data-generator/
├── data/
│   ├── pdf/          # Input PDF files
│   ├── html/         # Input HTML files
│   ├── youtube/      # YouTube URLs/transcripts
│   ├── docx/         # Input DOCX files
│   ├── ppt/          # Input PowerPoint files
│   ├── txt/          # Input text files
│   ├── output/       # Extracted text from inputs
│   ├── generated/    # Generated QA pairs and datasets
│   ├── cleaned/      # Quality-filtered datasets
│   └── final/        # Final formatted datasets
├── pyproject.toml    # Project dependencies
└── README.md         # This file
```

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- Poetry (for dependency management)
- Access to a Large Language Model (vLLM server or API endpoint)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd syntetic-data-generator
   ```

2. **Install dependencies**
   ```bash
   poetry install
   ```

3. **Set up your LLM backend**

   **Option A: Using vLLM (Recommended)**
   ```bash
   # Install vLLM
   pip install vllm
   
   # Start vLLM server (replace with your model)
   vllm serve meta-llama/Llama-3.3-70B-Instruct --port 8000
   ```

   **Option B: Using API endpoint**
   ```bash
   # Configure your API credentials in configs/custom_config.yaml
   ```

4. **Verify setup**
   ```bash
   poetry run synthetic-data-kit system-check
   ```

## 📖 Usage Examples

### Basic Workflow

1. **Ingest a document**
   ```bash
   poetry run synthetic-data-kit ingest data/pdf/research_paper.pdf
   ```

2. **Generate QA pairs**
   ```bash
   poetry run synthetic-data-kit create data/output/research_paper.txt --type qa
   ```

3. **Curate for quality**
   ```bash
   poetry run synthetic-data-kit curate data/generated/research_paper_qa_pairs.json
   ```

4. **Export to fine-tuning format**
   ```bash
   poetry run synthetic-data-kit save-as data/cleaned/research_paper_cleaned.json --format alpaca
   ```

### Advanced Examples

**Process a YouTube video:**
```bash
poetry run synthetic-data-kit ingest "https://www.youtube.com/watch?v=example"
poetry run synthetic-data-kit create data/output/youtube_example.txt --type cot
```

**Generate Chain-of-Thought reasoning:**
```bash
poetry run synthetic-data-kit create data/output/document.txt --type cot --num-pairs 30
```

**Batch processing multiple files:**
```bash
for file in data/pdf/*.pdf; do
  filename=$(basename "$file" .pdf)
  poetry run synthetic-data-kit ingest "$file"
  poetry run synthetic-data-kit create "data/output/${filename}.txt" -n 20
  poetry run synthetic-data-kit curate "data/generated/${filename}_qa_pairs.json"
  poetry run synthetic-data-kit save-as "data/cleaned/${filename}_cleaned.json" -f alpaca
done
```

## ⚙️ Configuration

The project uses the Meta Llama Synthetic Data Kit's configuration system. Create a `configs/config.yaml` file to customize:

```yaml
# Example configuration
llm:
  provider: "vllm"

vllm:
  api_base: "http://localhost:8000/v1"
  model: "meta-llama/Llama-3.3-70B-Instruct"

generation:
  temperature: 0.7
  chunk_size: 4000
  num_pairs: 25

curate:
  threshold: 7.0
  batch_size: 8
```

## 🎯 Use Cases

### AI Model Fine-tuning
Generate high-quality training data for:
- Domain-specific language models
- Specialized chatbots
- Code generation models
- Legal/medical AI assistants

### Testing & Validation
Create synthetic datasets for:
- Model performance evaluation
- A/B testing scenarios
- Edge case testing
- Robustness validation

### Research & Development
Produce data for:
- Algorithm development
- Benchmark creation
- Academic research
- Proof-of-concept demonstrations

## 🔧 Troubleshooting

### Common Issues

**vLLM Server Problems:**
- Ensure vLLM is properly installed: `pip install vllm`
- Check server status: `curl http://localhost:8000/health`
- Verify model accessibility and GPU memory

**Memory Issues:**
- Reduce batch size in configuration
- Use smaller models
- Adjust `--gpu-memory-utilization` in vLLM

**JSON Parsing Errors:**
- Install enhanced JSON parser: `pip install json5`
- Use verbose mode: `-v` flag
- Check LLM model JSON output capabilities

### Dependencies

Ensure all required parsers are installed:
```bash
pip install pdfminer.six beautifulsoup4 pytubefix youtube-transcript-api python-docx python-pptx
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Meta Llama Synthetic Data Kit](https://github.com/meta-llama/synthetic-data-kit) - The core library powering this project
- Meta AI Research team for developing the synthetic data generation techniques
- The open-source community for various supporting libraries

## 📞 Support

For issues related to:
- **This project**: Open an issue in this repository
- **Meta Llama Synthetic Data Kit**: Visit their [GitHub repository](https://github.com/meta-llama/synthetic-data-kit)

---

**Note**: This project is designed for educational and research purposes. Please ensure compliance with relevant data privacy and usage regulations when generating synthetic data.
