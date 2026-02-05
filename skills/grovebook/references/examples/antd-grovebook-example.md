# Ant Design Components Demo

This grovebook demonstrates various Ant Design (antd) components available in GraphXR Grove.

---

## Buttons

Antd provides several button types for different use cases.

<!--{"pinCode":false,"dname":"button-component","codeMode":"jsx","hide":false}-->
```jsx
function ButtonDemo() {
  const { Button, Space } = Antd;
  const [count, setCount] = React.useState(0);
  
  return (
    <Space direction="vertical" size="middle">
      <Space>
        <Button type="primary">Primary Button</Button>
        <Button>Default Button</Button>
        <Button type="dashed">Dashed Button</Button>
        <Button type="text">Text Button</Button>
        <Button type="link">Link Button</Button>
      </Space>
      <Space>
        <Button type="primary" onClick={() => setCount(count + 1)}>
          Clicked {count} times
        </Button>
        <Button danger>Danger Button</Button>
        <Button type="primary" disabled>Disabled</Button>
      </Space>
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"button-render","codeMode":"jsx","hide":false}-->
```jsx
react(<ButtonDemo />);
```

---

## Input Fields

Text inputs with various states and features.

<!--{"pinCode":false,"dname":"input-component","codeMode":"jsx","hide":false}-->
```jsx
function InputDemo() {
  const { Input, Space } = Antd;
  const [value, setValue] = React.useState('');
  
  return (
    <Space direction="vertical" size="middle" style={{ width: '100%' }}>
      <Input 
        placeholder="Type something..." 
        value={value}
        onChange={(e) => setValue(e.target.value)}
      />
      <div>You typed: <strong>{value || '(nothing yet)'}</strong></div>
      <Input.Password placeholder="Password input" />
      <Input.TextArea rows={3} placeholder="Multi-line text area" />
      <Input.Search placeholder="Search input" enterButton />
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"input-render","codeMode":"jsx","hide":false}-->
```jsx
react(<InputDemo />);
```

---

## Select Dropdown

Dropdown selection with single and multiple options.

<!--{"pinCode":false,"dname":"select-component","codeMode":"jsx","hide":false}-->
```jsx
function SelectDemo() {
  const { Select, Space } = Antd;
  const [selected, setSelected] = React.useState(null);
  
  const options = [
    { value: 'node', label: 'Node' },
    { value: 'edge', label: 'Edge' },
    { value: 'graph', label: 'Graph' },
    { value: 'category', label: 'Category' },
  ];
  
  return (
    <Space direction="vertical" size="middle" style={{ width: '100%' }}>
      <Select
        style={{ width: 200 }}
        placeholder="Select an item"
        options={options}
        onChange={(val) => setSelected(val)}
      />
      <div>Selected: <strong>{selected || '(none)'}</strong></div>
      <Select
        mode="multiple"
        style={{ width: '100%' }}
        placeholder="Select multiple items"
        options={options}
      />
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"select-render","codeMode":"jsx","hide":false}-->
```jsx
react(<SelectDemo />);
```

---

## Switch and Slider

Toggle switches and range sliders for value selection.

<!--{"pinCode":false,"dname":"switch-slider-component","codeMode":"jsx","hide":false}-->
```jsx
function SwitchSliderDemo() {
  const { Switch, Slider, Space } = Antd;
  const [enabled, setEnabled] = React.useState(false);
  const [sliderValue, setSliderValue] = React.useState(30);
  
  return (
    <Space direction="vertical" size="middle" style={{ width: '100%' }}>
      <Space>
        <Switch checked={enabled} onChange={setEnabled} />
        <span>Switch is: <strong>{enabled ? 'ON' : 'OFF'}</strong></span>
      </Space>
      <div>
        <div>Slider value: <strong>{sliderValue}</strong></div>
        <Slider 
          value={sliderValue} 
          onChange={setSliderValue}
          min={0}
          max={100}
        />
      </div>
      <div>
        <div>Range slider:</div>
        <Slider range defaultValue={[20, 50]} />
      </div>
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"switch-slider-render","codeMode":"jsx","hide":false}-->
```jsx
react(<SwitchSliderDemo />);
```

---

## Cards

Content containers with headers and actions.

<!--{"pinCode":false,"dname":"card-component","codeMode":"jsx","hide":false}-->
```jsx
function CardDemo() {
  const { Card, Space, Button } = Antd;
  
  return (
    <Space direction="vertical" size="middle" style={{ width: '100%' }}>
      <Card title="Basic Card" style={{ width: 300 }}>
        <p>This is a basic card with a title.</p>
        <p>Cards are great for grouping related content.</p>
      </Card>
      <Card 
        title="Card with Actions" 
        style={{ width: 300 }}
        extra={<a href="#">More</a>}
        actions={[
          <Button type="text" key="edit">Edit</Button>,
          <Button type="text" key="delete">Delete</Button>,
        ]}
      >
        <p>This card has header actions and footer actions.</p>
      </Card>
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"card-render","codeMode":"jsx","hide":false}-->
```jsx
react(<CardDemo />);
```

---

## Alerts

Feedback messages for different states.

<!--{"pinCode":false,"dname":"alert-component","codeMode":"jsx","hide":false}-->
```jsx
function AlertDemo() {
  const { Alert, Space } = Antd;
  
  return (
    <Space direction="vertical" size="middle" style={{ width: '100%' }}>
      <Alert message="Success message" type="success" showIcon />
      <Alert message="Info message" type="info" showIcon />
      <Alert message="Warning message" type="warning" showIcon />
      <Alert message="Error message" type="error" showIcon />
      <Alert
        message="Alert with description"
        description="This is a more detailed description of the alert message."
        type="info"
        showIcon
        closable
      />
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"alert-render","codeMode":"jsx","hide":false}-->
```jsx
react(<AlertDemo />);
```

---

## Progress and Spin

Loading indicators and progress bars.

<!--{"pinCode":false,"dname":"progress-component","codeMode":"jsx","hide":false}-->
```jsx
function ProgressDemo() {
  const { Progress, Spin, Space } = Antd;
  const [percent, setPercent] = React.useState(30);
  
  return (
    <Space direction="vertical" size="middle" style={{ width: '100%' }}>
      <Progress percent={percent} />
      <Progress percent={percent} status="active" />
      <Progress type="circle" percent={percent} width={80} />
      <Space>
        <Antd.Button onClick={() => setPercent(Math.max(0, percent - 10))}>-</Antd.Button>
        <Antd.Button onClick={() => setPercent(Math.min(100, percent + 10))}>+</Antd.Button>
      </Space>
      <Space>
        <Spin size="small" />
        <Spin />
        <Spin size="large" />
      </Space>
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"progress-render","codeMode":"jsx","hide":false}-->
```jsx
react(<ProgressDemo />);
```

---

## Tags and Badges

Labels and status indicators.

<!--{"pinCode":false,"dname":"tags-component","codeMode":"jsx","hide":false}-->
```jsx
function TagsDemo() {
  const { Tag, Badge, Space } = Antd;
  
  return (
    <Space direction="vertical" size="middle">
      <Space>
        <Tag>Default</Tag>
        <Tag color="success">Success</Tag>
        <Tag color="processing">Processing</Tag>
        <Tag color="error">Error</Tag>
        <Tag color="warning">Warning</Tag>
      </Space>
      <Space>
        <Tag color="magenta">magenta</Tag>
        <Tag color="red">red</Tag>
        <Tag color="volcano">volcano</Tag>
        <Tag color="orange">orange</Tag>
        <Tag color="gold">gold</Tag>
        <Tag color="lime">lime</Tag>
        <Tag color="green">green</Tag>
        <Tag color="cyan">cyan</Tag>
        <Tag color="blue">blue</Tag>
        <Tag color="purple">purple</Tag>
      </Space>
      <Space size="large">
        <Badge count={5}>
          <div style={{ width: 40, height: 40, background: '#ccc', borderRadius: 4 }} />
        </Badge>
        <Badge count={0} showZero>
          <div style={{ width: 40, height: 40, background: '#ccc', borderRadius: 4 }} />
        </Badge>
        <Badge dot>
          <div style={{ width: 40, height: 40, background: '#ccc', borderRadius: 4 }} />
        </Badge>
      </Space>
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"tags-render","codeMode":"jsx","hide":false}-->
```jsx
react(<TagsDemo />);
```

---

## Tooltip and Popover

Hover information displays.

<!--{"pinCode":false,"dname":"tooltip-component","codeMode":"jsx","hide":false}-->
```jsx
function TooltipDemo() {
  const { Tooltip, Popover, Button, Space } = Antd;
  
  const popoverContent = (
    <div>
      <p>This is the content of the popover.</p>
      <p>It can contain any React elements.</p>
    </div>
  );
  
  return (
    <Space size="large">
      <Tooltip title="This is a tooltip!">
        <Button>Hover me (Tooltip)</Button>
      </Tooltip>
      <Popover content={popoverContent} title="Popover Title">
        <Button>Hover me (Popover)</Button>
      </Popover>
      <Popover content={popoverContent} title="Click Popover" trigger="click">
        <Button>Click me (Popover)</Button>
      </Popover>
    </Space>
  );
}
```

<!--{"pinCode":false,"dname":"tooltip-render","codeMode":"jsx","hide":false}-->
```jsx
react(<TooltipDemo />);
```
