# PRODIGY_CS_02


from PIL import Image
import os

class ImageEncryptorDecryptor:
    def __init__(self, image_path):
        self.image_path = image_path
        self.img = Image.open(image_path)

    def encrypt(self, key):
        width, height = self.img.size
        encrypted_pixels = []

        for y in range(height):
            row = []
            for x in range(width):
                pixel = self.img.getpixel((x, y))
                encrypted_pixel = tuple((p + key) % 256 for p in pixel)
                row.append(encrypted_pixel)
            encrypted_pixels.append(row)

        encrypted_img = Image.new(self.img.mode, (width, height))
        for y in range(height):
            for x in range(width):
                encrypted_img.putpixel((x, y), encrypted_pixels[y][x])

        encrypted_img.save(os.path.join("D:\\6th_sem_code\\internship", "encrypted.png"))
        print("Image encrypted successfully!")

    def decrypt(self, key):
        encrypted_img = Image.open(self.image_path)
        width, height = encrypted_img.size
        decrypted_pixels = []

        for y in range(height):
            row = []
            for x in range(width):
                pixel = encrypted_img.getpixel((x, y))
                decrypted_pixel = tuple((p - key) % 256 for p in pixel)
                row.append(decrypted_pixel)
            decrypted_pixels.append(row)

        decrypted_img = Image.new(encrypted_img.mode, (width, height))
        for y in range(height):
            for x in range(width):
                decrypted_img.putpixel((x, y), decrypted_pixels[y][x])

        decrypted_img.save(os.path.join("D:\\6th_sem_code\\internship", "decrypted.png"))
        print("Image decrypted successfully!")


format_choice = input("Enter the format of the image (PNG or JPG): ").upper()


operation = input("Enter 'encrypt' or 'decrypt': ").lower()

if operation == "encrypt":
    image_name = input("Enter the name of the image file (e.g., xyz): ")
    image_path = os.path.join("D:\\6th_sem_code\\internship", f"{image_name}.{format_choice.lower()}")
    image_encryptor_decryptor = ImageEncryptorDecryptor(image_path)
    key = 150
    image_encryptor_decryptor.encrypt(key)
elif operation == "decrypt":
    image_name = input("Enter the name of the encrypted image file (e.g., xyz): ")
    image_path = os.path.join("D:\\6th_sem_code\\internship", f"{image_name}.{format_choice.lower()}")
    image_encryptor_decryptor = ImageEncryptorDecryptor(image_path)
    key = 150
    image_encryptor_decryptor.decrypt(key)
else:
    print("Invalid operation. Please enter 'encrypt' or 'decrypt'.")
