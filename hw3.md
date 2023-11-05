# 作業三
## 截圖
![image](https://github.com/bb-uraaka/y-1121-swiftui/assets/149889073/63156e8b-9dff-44c9-91ac-934ef9d9b292)

## 程式碼
```swift
import SwiftUI

extension Color {
    init(argb hex: Int) {
        let a = CGFloat(0xFF & (hex >> 24)) / 0xFF
        let r = CGFloat(0xFF & (hex >> 16)) / 0xFF
        let g = CGFloat(0xFF & (hex >> 8)) / 0xFF
        let b = CGFloat(0xFF & (hex >> 0)) / 0xFF
        self.init(uiColor: UIColor(red: r, green: g, blue: b, alpha: a))
    }

    init(_ rgb: Int) {
        let r = CGFloat(0xFF & (rgb >> 16)) / 0xFF
        let g = CGFloat(0xFF & (rgb >> 8)) / 0xFF
        let b = CGFloat(0xFF & (rgb >> 0)) / 0xFF
        self.init(uiColor: UIColor(red: r, green: g, blue: b, alpha: 1))
    }
}

struct 櫃子: View {   
    let border = Color(0xe7a75a)
    
    var body: some View {
            Rectangle()
                .fill(Color(0xf1f2f4))
            Rectangle()
                .fill(Color(0xad6d2d))
                .frame(width: 400, height: 400)
            Rectangle()
                .fill(.clear)
                .border(border, width: 10)
                .frame(width: 400, height: 400)
            Rectangle()
                .fill(.clear)
                .border(border, width: 10)
                .frame(width: 400, height: 10)
                .offset(y: -67)
            Rectangle()
                .fill(.clear)
                .border(border, width: 10)
                .frame(width: 400, height: 10)
                .offset(y: 67)
            Rectangle()
                .fill(.clear)
                .border(border, width: 10)
                .frame(width: 10, height: 400)
                .offset(x: -67)
            Rectangle()
                .fill(.clear)
                .border(border, width: 10)
                .frame(width: 10, height: 400)
                .offset(x: 67)
    }
}

struct Rounded: ViewModifier {
    let radius: CGFloat
    func body(content: Content) -> some View {
        content.clipShape(RoundedRectangle(cornerSize: CGSize(width: radius, height: radius)))
    }
}

extension View {
    func rounded(_ u: CGFloat) -> some View {
        modifier(Rounded(radius: u))
    }
    func padding(x: CGFloat = 0, y: CGFloat = 0) -> some View {
        padding(EdgeInsets(top: y, leading: x, bottom: y, trailing: x))
    }
}

struct ContentView: View {   
    let border = Color(0xe7a75a)

    var body: some View {
        ZStack {
            櫃子()
                .frame(width: 400, height: 400)
                .rounded(4)
            Text("很容易在附近滑倒的櫃子")
                .font(.system(size: 24))
                .padding(x: 8, y: 2)
                .background(Color(0xfae0be))
                .rounded(4)
                .shadow(radius: 2, y: 2)
                .offset(y: -250)
            VStack {
                HStack(spacing: 10) {
                    VStack {
                        Image("hachimitsu")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                        Text("蜂蜜幸運草")
                            .font(.system(size: 16))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                    }
                    .frame(width: 120, height: 120)
                    VStack {
                        Image("3gatsu")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                        Text("三月的獅子")
                            .font(.system(size: 16))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                    }
                    .frame(width: 120, height: 120)
                    VStack {
                        Image("furuba")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                        Text("魔法水果籃")
                            .font(.system(size: 16))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                    }
                    .frame(width: 120, height: 120)
                }
                HStack(spacing: 10) {
                    VStack {
                        Image("kimi_ni_todoke")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                            .padding(y: 8)
                        Text("只想告訴你")
                            .font(.system(size: 16))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                    }
                    .frame(width: 120, height: 120)
                    VStack {
                        Image("chihayafuru")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                            .padding(y: 4)
                        Text("夏目友人帳")
                            .font(.system(size: 16))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                    }
                    .frame(width: 120, height: 120)
                    VStack {
                        Spacer()
                        Image("aobuta")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                        Text("青春豬頭")
                            .font(.system(size: 16))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                    }
                    .frame(width: 120, height: 120)
                }
                HStack(spacing: 10) {
                    VStack {
                        Spacer()
                        Image("kazetsuyo")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                        Text("強風吹拂")
                            .font(.system(size: 16))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                    }
                    .frame(width: 120, height: 120)
                    VStack {
                        Spacer()
                        Image("kimiuso")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                        Text("四月是你的謊言")
                            .font(.system(size: 14))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                            .padding(y: 1)
                    }
                    .frame(width: 120, height: 120)
                    VStack {
                        Spacer()
                        Image("pm")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                        Text("可塑性記憶")
                            .font(.system(size: 16))
                            .padding(x: 8, y: 2)
                            .background(Color(0xfae0be))
                            .rounded(4)
                            .shadow(radius: 2)
                    }
                    .frame(width: 120, height: 120)
                }
            }
        }.ignoresSafeArea()
    }
}
```
