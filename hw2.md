# 作業二
## 影片
https://github.com/bb-uraaka/y-1121-swiftui/assets/149889073/5d967bfa-06b9-4653-8ecb-178267d68cd3

## 說明
使用者點選出拳後，app 會透過轉盤隨機決定電腦出拳，顯示輸贏狀況並計算比數。

- 利用 `GeometryReader` 讀取容器寬度與選項座標，就能知道要 offset 多少距離
- 使用 async function 搭配 `Task.sleep` 和 `withAnimation` 就能方便地寫出 procedural 動畫
- 使用 `Int.random` 可產生隨機亂數

## 程式碼
```swift
import SwiftUI

struct Action {
    let key: String
    let label: String
    let rotation: Int

    init(_ key: String, _ label: String, _ rotation: Int) {
        self.key = key
        self.label = label
        self.rotation = rotation
    }
}

let actions = [
    Action("scissors", "✌️", 0),
    Action("stone", "🤜", -95),
    Action("paper", "✋", 0),
]

struct ContentView: View {
    @State var playerScore = 0
    @State var computerScore = 0
    @State var selectedAction = ""
    @State var rotationDegrees = 0.0
    @State var judgement = ""
    
    func sleep(_ seconds: Double) async {
        do {
            let gigaSeconds = Int(pow(10, Double(9)))
            let ns = Double(gigaSeconds) * seconds
            try await Task.sleep(nanoseconds: UInt64(ns))
        } catch {}
    }

    func actionOnTap(_ key: String) async {
        if (selectedAction != "") {
            return
        }
        withAnimation(.easeOut) {
            selectedAction = key
        }
        await sleep(0.5)
        let computer = Int.random(in: 0...2)
        let player = actions.firstIndex(where: { ac in ac.key == key })!
        let computerWins = computer == (player + 1) % 3
        let draw = computer == player
        let deg = 360 * 3 + 120 * computer
        withAnimation(.linear(duration: 1)) {
            rotationDegrees = Double(deg)
        }
        await sleep(1)
        withAnimation(.easeOut(duration: 0.5)) {
            judgement = draw ? "平手" : computerWins ? "電腦贏了" : "你贏了"
        }
        await sleep(1.5)
        withAnimation(.easeOut(duration: 0.5)) {
            judgement = ""
            selectedAction = ""
        }
        await sleep(0.5)
        rotationDegrees = 0
        withAnimation {
            if (!draw) {
                if (computerWins) {
                    computerScore += 1
                } else {
                    playerScore += 1
                }
            }
        }
    }

    var body: some View {
        let selectedAnyAction = selectedAction != ""
        ZStack {
            VStack {
                ZStack {
                    Circle()
                        .fill(.white)
                        .clipShape(Circle())
                        .frame(width: 250, height: 250)
                        .overlay(Circle().stroke(.gray, lineWidth: 3))
                        .overlay(Path { path in
                            let center = CGPoint(x: 250 / 2, y: 250 / 2)
                            let top = CGPoint(x: 250 / 2, y: 0)
                            let r = CGFloat(250 / 2)
                            path.move(to: center)
                            path.addLine(to: top)
                            path.addArc(center: center, radius: r, startAngle: .degrees(-90), endAngle: .degrees(30), clockwise: true)
                            path.addLine(to: center)
                            path.addArc(center: center, radius: r, startAngle: .degrees(30), endAngle: .degrees(150), clockwise: true)
                            path.addLine(to: center)
                        }.stroke(.gray, lineWidth: 2.5))
                        .overlay(Text("🤜").offset(x: 60, y: -30))
                        .overlay(Text("✌️").offset(x: 0, y: 60))
                        .overlay(Text("✋").offset(x: -60, y: -30))
                }
                .rotationEffect(.degrees(rotationDegrees))
                .font(.system(size: 64))
                .padding(30)
                Text("⬆️")
                    .font(.system(size: 42))
                Text(judgement)
                    .font(.system(size: 28))
                    .padding(10)
                Spacer()
            }.frame(maxWidth: .infinity)
                .opacity(selectedAnyAction ? 1 : 0)
            VStack(spacing: 100) {
                HStack(spacing: 64) {
                    VStack {
                        Text("\(playerScore)")
                            .font(
                                .system(size: 64, weight: .medium)
                                .monospacedDigit()
                            )
                        Text("你")
                    }
                    VStack {
                        Text("\(computerScore)")
                            .font(
                                .system(size: 64, weight: .medium)
                                .monospacedDigit()
                            )
                        Text("電腦")
                    }
                }
                .font(.system(size: 32))
                .opacity(selectedAnyAction ? 0 : 1)
                GeometryReader(content: { groupGeo in
                    let containerRect = CGRect(origin: .zero, size: groupGeo.size)
                    HStack(spacing: 48) {
                        ForEach(actions, id: \.key) { action in
                            GeometryReader(content: { geo in
                                let selected = selectedAction == action.key
                                let shouldHide = selectedAnyAction && !selected
                                let offsetX = containerRect.midX - geo.frame(in: .named("actions")).midX
                                ZStack {
                                    Text(action.label)
                                        .rotationEffect(.degrees(selected ? Double(action.rotation) : 0))
                                        .onTapGesture {
                                            Task {
                                                await actionOnTap(action.key)
                                            }
                                        }
                                        .opacity(shouldHide ? 0 : 1)
                                }
                                .offset(x: selected ? offsetX : 0, y: selected ? 80 : 0)
                            })
                        }
                    }
                    .coordinateSpace(name: "actions")
                    .font(.system(size: 64))
                })
            }.frame(width: 300, height: 300)
        }
        .frame(maxWidth: .infinity, maxHeight: .infinity)
        .background(Color(red: 0.949, green: 0.9215, blue: 0.81568, opacity: 0.5))
    }
}
```
