# PowerPoint MCP Server

A Model Context Protocol (MCP) server for creating and manipulating PowerPoint presentations with AI assistance. This server provides a comprehensive API for AI models to interact with PowerPoint files, supporting advanced formatting, financial charts, and data integration.

## Features

### Presentation Management
- Create and modify PowerPoint presentations
- Add, delete, and modify slides
- Save and load presentations from workspace
- Template management system

### Element Operations
- Fine-grained control over slide elements (text, shapes, images, charts)
- Advanced shape creation and styling
- Element positioning and grouping
- Connector lines between shapes

### Financial Integration
- Create financial charts (line, bar, column, pie, waterfall, etc.)
- Generate comparison tables
- Support for various financial metrics:
  - Revenue
  - EBITDA
  - Profit
  - Assets
  - Equity
  - Growth rates
  - Margins
- Currently uses dummy data, with plans to integrate Proff API for Norwegian company data
- Adaptable to other financial data providers through API customization

### Styling and Formatting
- Rich text formatting
- Shape styling (fills, gradients, outlines)
- Chart customization
- Background colors and effects

## Installation

1. Clone the repository:
```bash
git clone https://github.com/jenstangen1/pptx-mcp.git
cd pptx-mcp
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

## Setting up with Claude

### Installing the MCP into Claude's Interface

To integrate this PowerPoint MCP with Claude, add the following JSON configuration to your Claude MCP file:

```json
{
    "mcpServers": {
      "powerpoint_mcp": {
        "command": "uv",
        "args": [
          "--directory",
          "your directory here",
          "run",
          "mcp_powerpoint_server.py"
        ]
      }
    }
}
```

Note: You may need to modify the directory path to match your installation location.

## Available MCP Tools

### Presentation Management
- `list_presentations`: List all PowerPoint files in the workspace
- `upload_presentation`: Upload a new presentation to the workspace
- `save_presentation`: Save the current presentation

### Slide Operations
- `add_slide`: Add a new slide to the presentation
- `delete_slide`: Delete a slide from the presentation
- `get_slide_count`: Get the total number of slides in the presentation
- `analyze_slide`: Analyze the content of a slide
- `set_background_color`: Set the background color of a slide

### Element Operations
- `add_text`: Add text to a slide
- `add_shape`: Add a shape to a slide
- `edit_element`: Edit an element's properties
- `style_element`: Apply styling to an element
- `connect_shapes`: Connect two shapes with a connector
- `find_element`: Find elements on a slide based on criteria

### Financial Tools
- `get_company_financials`: Get financial data for a company (currently returns dummy data)
- `create_financial_chart`: Create a financial chart on a slide
- `create_comparison_table`: Create a comparison table for companies

**Note:** The financial tools currently use dummy data. Future versions plan to integrate with the Proff API for automatic fetching of Norwegian company data. Users can modify the code to connect with local or preferred financial data providers.

### Template Operations
- `list_templates`: List all available templates
- `apply_template`: Apply a template to a presentation
- `create_slide_from_template`: Create a new slide from a template
- `save_as_template`: Save a slide as a template

### Debug Tools
- `debug_element_mappings`: Debug tool to inspect element mappings for a slide

## Usage

### Starting the Server

Run the server:
```bash
python mcp_powerpoint_server.py
```

The server will create a workspace directory for presentations and templates if they don't exist.

### Basic Operations

```python
# List presentations
presentations = mcp.list_presentations()

# Create a new slide
slide_index = mcp.add_slide(presentation_path, layout_name="Title and Content")

# Add text to a slide
element_id = mcp.add_text(
    presentation_path=presentation_path,
    slide_index=slide_index,
    text="Hello World",
    position=[1.0, 1.0],
    font_size=24
)

# Add a shape
shape_id = mcp.add_shape(
    presentation_path=presentation_path,
    slide_index=slide_index,
    shape_type="rectangle",
    position={"x": 2.0, "y": 2.0},
    size={"width": 2.0, "height": 1.0}
)
```

### Financial Charts

```python
# Create a financial chart
chart_id = mcp.create_financial_chart(
    presentation_path=presentation_path,
    slide_index=slide_index,
    chart_type="column",
    data={
        "categories": ["2020", "2021", "2022"],
        "series": [{
            "name": "Revenue",
            "values": [1000000, 1200000, 1500000]
        }]
    },
    position={"x": 1.0, "y": 1.0},
    size={"width": 6.0, "height": 4.0},
    title="Revenue Growth"
)

# Create a comparison table
table_id = mcp.create_comparison_table(
    presentation_path=presentation_path,
    slide_index=slide_index,
    companies=["Company A", "Company B"],
    metrics=["revenue", "ebitda", "margin"],
    position={"x": 1.0, "y": 1.0},
    title="Company Comparison"
)
```

### Template Management

```python
# List available templates
templates = mcp.list_templates()

# Apply a template
mcp.apply_template(
    presentation_path=presentation_path,
    template_name="financial_report",
    options={
        "apply_master": True,
        "apply_theme": True,
        "apply_layouts": True
    }
)

# Create a slide from template
mcp.create_slide_from_template(
    presentation_path=presentation_path,
    template_name="comparison_slide",
    content={
        "title": "Market Analysis",
        "subtitle": "Q3 2023"
    }
)
```

## Directory Structure

```
pptx-mcp/
├── mcp_powerpoint_server.py  # Main server implementation
├── requirements.txt          # Python dependencies
├── presentations/           # Workspace for presentations
│   └── templates/          # Template storage
└── README.md               # This file
```

## Dependencies

- python-pptx: PowerPoint file manipulation
- Pillow: Image processing
- numpy: Numerical operations
- MCP SDK: Model Context Protocol implementation

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.