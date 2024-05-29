import fitz  # PyMuPDF
from PIL import Image

def pdf_to_images(pdf_path, output_folder, dpi=300):
    # PDFファイルを開く
    pdf_document = fitz.open(pdf_path)

    # 各ページを画像に変換する
    for page_num in range(len(pdf_document)):
        page = pdf_document.load_page(page_num)
        matrix = fitz.Matrix(dpi / 72, dpi / 72)  # DPIを設定
        pix = page.get_pixmap(matrix=matrix, alpha=False)

        # 画像ファイル名を設定
        img_filename = f"{output_folder}/page_{page_num + 1}.png"

        # 画像を保存
        image = Image.frombytes("RGB", [pix.width, pix.height], pix.samples)
        image.save(img_filename)

        print(f"Saved {img_filename}")

# 使用例
pdf_path = "example.pdf"  # 入力するPDFファイルのパス
output_folder = "output_images"  # 出力画像を保存するフォルダーのパス

# 出力フォルダーが存在しない場合は作成
import os
if not os.path.exists(output_folder):
    os.makedirs(output_folder)

pdf_to_images(pdf_path, output_folder, dpi=300)
