# Cypress React Order Status Selector Component Test

This Cypress component test verifies the behavior of the OrderStatusSelector component, which uses Radix UI Select to allow users to choose an order status (New, Processed, Fulfilled).

The tests ensure that:

The selector renders with a default value of New.

Selecting Processed updates the displayed value and triggers the onChange callback with the correct status.

Selecting Fulfilled updates the displayed value and triggers the onChange callback with the correct status.

The component is wrapped inside the Radix <Theme> provider to provide the necessary context for Radix UI components.

## React Order Status Selector Component



```bash
import { Select } from "@radix-ui/themes";

interface Props {
  onChange: (status: string) => void;
}

const OrderStatusSelector = ({ onChange }: Props) => {
  return (
    <Select.Root defaultValue="new" onValueChange={onChange}>
      <Select.Trigger />
      <Select.Content>
        <Select.Group>
          <Select.Label>Status</Select.Label>
          <Select.Item value="new">New</Select.Item>
          <Select.Item value="processed">Processed</Select.Item>
          <Select.Item value="fulfilled">Fulfilled</Select.Item>
        </Select.Group>
      </Select.Content>
    </Select.Root>
  );
};

export default OrderStatusSelector;


```


## Cypress Component Test



```bash
import { mount } from "cypress/react";
import { Theme } from "@radix-ui/themes";
import OrderStatusSelector from "../../src/components/OrderStatusSelector";

describe("OrderStatusSelector Component", () => {
  const mountWithTheme = (onChange = cy.stub().as("onChange")) => {
    mount(
      <Theme>
        <OrderStatusSelector onChange={onChange} />
      </Theme>
    );
    return onChange;
  };

  it("renders with default value 'new'", () => {
    mountWithTheme();
    cy.get('[role="combobox"]').should("contain.text", "New");
  });

  it("allows selecting 'Processed' and calls onChange", () => {
    mountWithTheme();

    cy.get('[role="combobox"]').click();
    cy.contains("Processed").click();

    cy.get('[role="combobox"]').should("contain.text", "Processed");
    cy.get("@onChange").should("have.been.calledWith", "processed");
  });

  it("allows selecting 'Fulfilled' and calls onChange", () => {
    const onChange = mountWithTheme();

    cy.get('[role="combobox"]').click();
    cy.contains("Fulfilled").click();

    cy.get('[role="combobox"]').should("contain.text", "Fulfilled");
    cy.get("@onChange").should("have.been.calledWith", "fulfilled");
  });
});

```

![Screenshot of Label Component](Screenshot%202025-09-27%20233639.png)

| Test Case                                           | Purpose                                                        | Expected Outcome                                                             |
| --------------------------------------------------- | -------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **renders with default value 'new'**                | Verify that the selector initializes with `New` selected       | The combobox should display **New**                                          |
| **allows selecting 'Processed' and calls onChange** | Check that selecting `Processed` updates UI and calls callback | The combobox shows **Processed** and `onChange` is called with `"processed"` |
| **allows selecting 'Fulfilled' and calls onChange** | Check that selecting `Fulfilled` updates UI and calls callback | The combobox shows **Fulfilled** and `onChange` is called with `"fulfilled"` |

