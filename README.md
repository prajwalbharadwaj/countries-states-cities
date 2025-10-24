# Countries, States & Cities Explorer

A modern Vue.js application that provides an interactive interface to explore countries, their states, and cities with real-time JSON data visualization editor.

## 🌟 Features

- **Interactive Dropdown Selection**: Cascading dropdowns for countries, states, and cities
- **Real-time JSON Visualization**: Live JSON preview using CodeMirror editor
- **Search Functionality**: Search through countries, states, and cities
- **Responsive Design**: Mobile-friendly interface with adaptive layouts
- **Modern UI**: Clean, intuitive interface with smooth animations
- **Comprehensive Data**: Complete dataset of countries, states, and cities worldwide

## 🚀 Quick Start

1. Start the development server:

```bash
pnpm dev
# or
npm run dev
```

2. Open your browser and navigate to `http://localhost:8080`

## 🛠️ Built With

- **Vue 3** - Progressive JavaScript framework
- **Vite** - Fast build tool and development server
- **CodeMirror 6** - Powerful code editor for JSON visualization
- **VueUse** - Collection of Vue composition utilities
- **CSS3** - Modern styling with responsive design

## 📁 Project Structure

```
src/
├── assets/
│   ├── csc.json          # Countries, states, and cities data
│   └── main.css          # Global styles
├── components/
│   ├── CodeMirrorEditor.vue  # JSON code editor component
│   ├── Dropdown.vue          # Reusable dropdown component
│   └── Footer.vue            # Footer component
├── App.vue               # Main application component
└── main.js              # Application entry point
```

## 🎯 Usage

1. **Select a Country**: Choose from the dropdown to see available states
2. **Select a State**: Pick a state to view its cities
3. **Select a City**: Choose a city to see its details
4. **View JSON Data**: Each selection displays the corresponding JSON data in the editor

## 📱 Responsive Design

The application is fully responsive and works seamlessly across:

- Desktop (768px and above)
- Tablet (768px and below)
- Mobile devices

### Data

- The countries, states, and cities data is stored in `src/assets/csc.json`
- The data structure includes comprehensive information like:
  - Country codes (ISO2, ISO3)
  - Currency information
  - Timezone data
  - Geographic coordinates
  - Translations in multiple languages

## 📞 Support

If you have any questions or need help, please open an issue on GitHub.

---

Made with ❤️ using Vue 3 and CodeMirror
