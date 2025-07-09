# Tổng hợp Pandoc

Pandoc để chuyển đổi tệp. Hết....


### Markdown -> EPUB

```
pandoc output.md -o output.epub --metadata title="Book Title"
```

### DOCX -> PDF

```
pandoc -i file.docx -o file.pdf
```

Để chuyển sang PDF thì cần có `pdf-latex` gì đó nên mình gợi ý tải LibreOffice.