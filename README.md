# swiftui_side_bars

### Version1 

```swift

struct MainView: View {
    
    @State var selectedTab = "Home"
    @State var showMenu: Bool = false
    @Namespace var animation
    
    var body: some View {
        ZStack {
            Color.blue.ignoresSafeArea()
            
            SideMenu(selectedTab: self.$selectedTab)
        
            ZStack {
                
                // Two Background Cards....
                Color.white
                    .opacity(0.5)
                    .cornerRadius(self.showMenu ? 15 : 0)
                    // shadow...
                    .shadow(color: Color.black.opacity(0.07), radius: 5, x: -5, y: 0)
                    .offset(x: self.showMenu ? -25 : 0)
                    .padding(.vertical, 30)
                
                Color.white
                    .opacity(0.5)
                    .cornerRadius(self.showMenu ? 15 : 0)
                    // shadow...
                    .shadow(color: Color.black.opacity(0.07), radius: 5, x: -5, y: 0)
                    .offset(x: self.showMenu ? -50 : 0)
                    .padding(.vertical, 60)
                
                AppTabView(selectedTab: self.$selectedTab)
                    .cornerRadius(self.showMenu ? 15 : 0)
            } //: ZSTACK
            .scaleEffect(self.showMenu ? 0.84 : 1)
            .offset(x: self.showMenu ? UIScreen.main.bounds.width - 120 : 0)
            .ignoresSafeArea()
            .overlay(alignment: .topLeading) {
                Button {
                    withAnimation(.spring()) {
                        self.showMenu.toggle()
                    }
                } label: {
                    VStack(spacing: 5) {
                        Capsule()
                            .fill(self.showMenu ? Color.white : Color.primary)
                            .frame(width: 30, height: 3)
                            .rotationEffect(.init(degrees: self.showMenu ? -50 : 0))
                            .offset(x: self.showMenu ? 2 : 0, y: self.showMenu ? 9 : 0)
                        
                        VStack(spacing: 5) {
                            Capsule()
                                .fill(self.showMenu ? Color.white : Color.primary)
                                .frame(width: 30, height: 3)
                            
                            Capsule()
                                .fill(self.showMenu ? Color.white : Color.primary)
                                .frame(width: 30, height: 3)
                                .offset(y: self.showMenu ? -8 : 0)
                        } //: VSTACK
                        .rotationEffect(.init(degrees: self.showMenu ? 50 : 0))
                    } //: VSTACK
                    
          
                }
                .padding()
            }
            
        
        }
    } //: BODY
    
}

struct SideMenu: View {
    
    @Binding var selectedTab: String
    @Namespace var animation
    
    var body: some View {
        ZStack {
            Color.blue
                .ignoresSafeArea()
            
            // Side Menu...
            VStack(alignment: .leading, spacing: 15) {
                
                Image("img1")
                    .resizable()
                    .aspectRatio(contentMode: .fill)
                    .frame(width: 70, height: 70)
                    .cornerRadius(10)
                // Padding top for Top Close Button...
                    .padding(.top, 50)
                
                VStack(alignment: .leading, spacing: 6) {
                    Text("Jenna Ezarik")
                        .font(.title)
                        .fontWeight(.heavy)
                        .foregroundColor(.white)
                    
                    Button {
                        
                    } label: {
                        Text("View Profile")
                            .fontWeight(.semibold)
                            .foregroundColor(.white)
                            .opacity(0.7)
                    }

                } //: VSTACK
                
                
                // Tab Buttons...
                VStack(alignment: .leading, spacing: 10) {
                    TabButton(image: "house",
                              title: "Home",
                              selectedTab: self.$selectedTab,
                              animation: self.animation)
                    
                    TabButton(image: "clock.arrow.circlepath",
                              title: "History",
                              selectedTab: self.$selectedTab,
                              animation: self.animation)
                    
                    TabButton(image: "bell.badge",
                              title: "Notifications",
                              selectedTab: self.$selectedTab,
                              animation: self.animation)
                    
                    TabButton(image: "gearshape.fill",
                              title: "Settings",
                              selectedTab: self.$selectedTab,
                              animation: self.animation)
                    
                    TabButton(image: "questionmark.circle",
                              title: "Help",
                              selectedTab: self.$selectedTab,
                              animation: self.animation)
                } //: VSTACK
                .padding(.leading, -15)
                .padding(.top, 50)
                
                Spacer()
                
                // Sign Out Button...
                VStack(alignment: .leading, spacing: 6, content: {
                    TabButton(image: "rectangle.righthalf.inset.fill.arrow.right",
                              title: "Log out",
                              selectedTab: .constant(""),
                              animation: self.animation)
                    .padding(.leading, -15)
                    
                    Text("App Version 1.2.34")
                        .font(.caption)
                        .fontWeight(.semibold)
                        .foregroundColor(.white)
                        .opacity(0.6)
                })
                
            } //: VSTACK
            .padding()
            .frame(maxWidth: .infinity, maxHeight: .infinity, alignment: .topLeading)
        } //: ZSTACK

    }
    
}

struct TabButton: View {
    var image: String
    var title: String
    
    @Binding var selectedTab: String
    
    // For Hero Animation Slide...
    var animation: Namespace.ID
    
    var body: some View {
        Button(action: {
            withAnimation(.spring()) {
                self.selectedTab = self.title
            }
        } , label: {
            HStack(spacing: 15) {
                Image(systemName: self.image)
                    .font(.title2)
                    .frame(width: 30)
                
                Text(self.title)
                    .fontWeight(.semibold)
            } //: HSTACK
            .foregroundColor(self.selectedTab == title ? Color.blue : .white)
            .padding(.vertical, 12)
            .padding(.horizontal, 20)
            // Max Frame...
            .frame(width: UIScreen.main.bounds.width - 170, alignment: .leading)
            .background(
                // hero animation...
                ZStack {
                    if self.selectedTab == self.title {
                        Color.white
                            .opacity(self.selectedTab == self.title ? 1 : 0)
                            .clipShape(CustomCorners(corners: [.topRight, .bottomRight], radius: 10))
                            .matchedGeometryEffect(id: "TAB", in: self.animation)
                    }
                }
            )
        })
    }
    
}

struct CustomCorners: Shape {
    
    var corners: UIRectCorner
    var radius: CGFloat
    
    func path(in rect: CGRect) -> Path {
        let path = UIBezierPath(roundedRect: rect, byRoundingCorners: corners, cornerRadii: CGSize(width: self.radius, height: self.radius))
        return Path(path.cgPath)
    }
    
}

struct AppTabView: View {
    
    @Binding var selectedTab: String
    
    var body: some View {
        // Tab View With Tabs...
        TabView(selection: self.$selectedTab) {
            Home()
                .tag("Home")
            History()
                .tag("History")
            Notification()
                .tag("Notification")
            Settings()
                .tag("Settings")
            Help()
                .tag("Help")
        }
    }
    
}

struct Home: View {
    
    
    var body: some View {
        NavigationStack {
            Text("Home")
                .font(.largeTitle)
                .fontWeight(.heavy)
                .foregroundColor(.primary)
                .navigationTitle("Home")
        }
    }
    
}

struct History: View {
    
    
    var body: some View {
        NavigationStack {
            Text("History")
                .font(.largeTitle)
                .fontWeight(.heavy)
                .foregroundColor(.primary)
                .navigationTitle("History")
        }
    }
    
}

struct Notification: View {
    
    
    var body: some View {
        NavigationStack {
            Text("Notification")
                .font(.largeTitle)
                .fontWeight(.heavy)
                .foregroundColor(.primary)
                .navigationTitle("Notification")
        }
    }
    
}

struct Settings: View {
    
    
    var body: some View {
        NavigationStack {
            Text("Settings")
                .font(.largeTitle)
                .fontWeight(.heavy)
                .foregroundColor(.primary)
                .navigationTitle("Settings")
        }
    }
    
}

struct Help: View {
    
    
    var body: some View {
        NavigationStack {
            Text("Helps")
                .font(.largeTitle)
                .fontWeight(.heavy)
                .foregroundColor(.primary)
                .navigationTitle("Helps")
        }
    }
    
}

```
