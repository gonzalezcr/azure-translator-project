# azure-translator-project
🌐 Tradutor de Artigos Técnicos com Azure AI

Você já se perguntou como traduzir documentos ou páginas web de maneira eficiente e personalizada?
O Azure Translator oferece um conjunto robusto de ferramentas para tradução automática que podem ser ajustadas às suas necessidades específicas.

Este projeto demonstra como utilizar essas ferramentas com exemplos práticos em Python.

📌 Visão Geral
O Azure AI disponibiliza dois serviços principais para tradução:

1️⃣ API REST de Tradução Multilíngue
Permite traduzir textos dinamicamente através de requisições HTTP.

Recursos:
Detecção automática de idioma
Tradução para múltiplos idiomas simultaneamente
Transliteração de escrita (ex.: japonês → romaji)

Fluxo de funcionamento
Texto → Detecção de idioma → Tradução → (Opcional) Transliteração

2️⃣ Tradução Personalizada
Permite criar modelos próprios treinados com seus dados.

Passos para criar um modelo customizado:
Configurar modelo no portal de tradutor personalizado
Vincular workspace ao recurso de tradutor
Enviar dados de treinamento
Treinar modelo
Chamar API usando o parâmetro category

⚙️ Pré-requisitos

Instale as dependências:
pip install requests python-docx beautifulsoup4

Você também precisará de:
Chave de API do Azure
Endpoint do serviço
Região do recurso

Configuração da API
subscription_key = "SUA_CHAVE_API"
endpoint = "https://api.cognitive.microsofttranslator.com"
location = "eastus"
language_destination = "pt-br"

🔤 Função Base de Tradução

Esta função envia texto para a API e retorna a tradução.

import requests
def translator_text(text, target_language):
    headers = {
        "Ocp-Apim-Subscription-Key": subscription_key,
        "Content-type": "application/json"
    }
    params = {
        "api-version": "3.0",
        "from": "en",
        "to": target_language
    }
    body = [{"text": text}]
    response = requests.post(
        endpoint + "/translate",
        params=params,
        headers=headers,
        json=body
    )
    return response.json()[0]["translations"][0]["text"]

📄 Tradução de Documentos .docx
Script que lê um documento Word, traduz o conteúdo e salva um novo arquivo.

from docx import Document
def translate_document(path):
    document = Document(path)
    translated_doc = Document()
    for paragraph in document.paragraphs:
        translated_text = translator_text(paragraph.text, language_destination)
        translated_doc.add_paragraph(translated_text)
    translated_doc.save(path.replace(".docx", "_translated.docx"))

Uso
translate_document("arquivo.docx")

🌍 Tradução de Página Web
Esse script:

Baixa a página
Extrai textos
Traduz conteúdo
Salva resultado em .docx

from bs4 import BeautifulSoup
from docx import Document
import requests
def fetch_and_translate(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, "html.parser")
    text = "\n".join([p.get_text() for p in soup.find_all("p")])
    translated_text = translator_text(text, language_destination)
    doc = Document()
    doc.add_paragraph(translated_text)
    doc.save("translated_webpage.docx")

Uso
fetch_and_translate("https://exemplo.com")

🧠 Possibilidades de Expansão
Você pode evoluir este projeto adicionando:

Interface gráfica
Tradução em lote de múltiplos arquivos
Upload automático de PDFs
Integração com banco de dados
Tradução em tempo real via streaming

📚 Conclusão

Com o Azure Translator é possível criar soluções poderosas de tradução automática para documentos, sites e sistemas corporativos.
A API é simples, flexível e permite desde usos básicos até modelos personalizados treinados com seus próprios dados.

----------------------------------------------------------------------------------------------------------------------------------
Artigo baseado no conteúdo do desafio:
https://www.dio.me/articles/tradutor-de-artigos-tecnicos-com-azure-ai
