import os
import random
from io import BytesIO

# import imageio
from PIL import Image, ImageDraw, ImageFont


class GifCodeImage(object):
    def __init__(self, width=200, height=75, code_count=4, font_size=32, point_count=20, line_count=3, frame_count=2):
        """
        :param width: 图片宽度
        :param height: 图片高度
        :param code_count: 验证码位数
        :param font_size: 字体大小
        :param point_count: 噪点数量
        :param line_count: 噪线数量
        :param frame_count: gif的帧数
        """
        self.width = width
        self.height = height
        self.code_count = code_count
        self.font_size = font_size
        self.point_count = point_count
        self.line_count = line_count
        self.frame_count = frame_count

    @staticmethod
    def get_random_color():
        """
        获取一个随机颜色(r,g,b)格式的
        :return: c1, c2, c3
        """
        c1 = random.randint(0, 255)
        c2 = random.randint(0, 255)
        c3 = random.randint(0, 255)
        return c1, c2, c3

    @staticmethod
    def get_random_str():
        """
        获取一个随机字符串，每个字符的颜色也是随机的
        :return: random_char
        """
        random_num = str(random.randint(0, 9))
        random_low_alpha = chr(random.randint(97, 122))
        random_upper_alpha = chr(random.randint(65, 90))
        random_char = random.choice([random_num, random_low_alpha, random_upper_alpha])
        return random_char

    def get_code_and_image(self):
        """
        生成验证码与动画帧数
        :return: code_str
        """
        code_str_list = []
        for _ in range(self.code_count):
            s = self.get_random_str()
            code_str_list.append(s)
        bg_color = self.get_random_color()
        frames = []
        for item in range(self.frame_count):
            image = Image.new('RGB', (self.width, self.height), bg_color)
            draw = ImageDraw.Draw(image)
            path = os.path.join(os.getcwd(), "JetBrainsMono-BoldItalic.ttf")
            font = ImageFont.truetype(path, size=self.font_size)
            for i, code in enumerate(code_str_list):
                v = random.randint(0, 10)
                x = random.randint(2, 22)
                draw.text((x + i * 40, v), code, self.get_random_color(), font=font)

            # 噪点噪线，划线
            for i in range(self.line_count):
                x1 = random.randint(0, self.width)
                x2 = random.randint(0, self.width)
                y1 = random.randint(0, self.height)
                y2 = random.randint(0, self.height)
                draw.line((x1, y1, x2, y2), fill=self.get_random_color())

            # 画点
            for i in range(self.point_count):
                draw.point([random.randint(0, self.width), random.randint(0, self.height)],
                           fill=self.get_random_color())
                x = random.randint(0, self.width)
                y = random.randint(0, self.height)
                draw.arc((x, y, x + 4, y + 4), 0, 90, fill=self.get_random_color())
            # 
            buffer = BytesIO()
            image.save(buffer, "png")
            frames.append(Image.open(buffer))
            #
            # 注释的部分是配合imageio使用的，以上做切换
            # data = buffer.getvalue()
            # buffer.close()
            # data = imageio.imread(data, format="png")
            # frames.append(data)

        frames[0].save('code.gif', format='GIF', append_images=frames[1:], save_all=True, duration=350, loop=0)
        #  imageio.mimsave("code.gif", frames, 'GIF', duration=350)
        return "".join(code_str_list)


if __name__ == "__main__":
    img = GifCodeImage(frame_count=20)
    code_str = img.get_code_and_image()
    print(code_str)
