# ShadowTrace
shadowtrace.py
import PyPDF2
import nltk
from nltk.tokenize import sent_tokenize


class ShadowTrace:

    def __init__(self):
        self.suspicious_keywords = [
            "confidential", "secret", "password", "unauthorized",
            "fraud", "bank", "leak", "breach", "illegal", "hidden"
        ]

    def extract_text(self, file_path):
        """Extract text from PDF or TXT"""
        try:
            if file_path.endswith(".txt"):
                with open(file_path, "r", encoding="utf-8") as f:
                    return f.read()

            elif file_path.endswith(".pdf"):
                reader = PyPDF2.PdfReader(file_path)
                text = ""
                for page in reader.pages:
                    text += page.extract_text()
                return text

            else:
                return "Unsupported file format."

        except Exception as e:
            return f"Error reading file: {e}"

    def detect_anomalies(self, text):
        """Detect suspicious words"""
        found = []
        for word in self.suspicious_keywords:
            if word.lower() in text.lower():
                found.append(word)
        return found

    def summarize(self, text):
        """Simple summary (first 2 sentences)"""
        sentences = sent_tokenize(text)
        if len(sentences) >= 2:
            return sentences[:2]
        return sentences

    def run_shadowtrace(self, file_path):
        print("\n🔍 ShadowTrace – Document Intelligence Agent")
        print("============================================")

        text = self.extract_text(file_path)

        if "Error" in text:
            print(text)
            return

        print("\n📄 Extracted Text (first 200 characters):")
        print(text[:200], "...")

        anomalies = self.detect_anomalies(text)
        summary = self.summarize(text)

        print("\n⚠️ Detected Anomalies:")
        if anomalies:
            print(", ".join(anomalies))
        else:
            print("No suspicious activity found.")

        print("\n📝 Summary:")
        for line in summary:
            print("-", line)


# Run the project
if __name__ == "__main__":
    file_path = input("Enter PDF/TXT file path: ")
    agent = ShadowTrace()
    agent.run_shadowtrace(file_path)
