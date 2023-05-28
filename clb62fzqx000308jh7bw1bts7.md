---
title: "Build a modern user interface with ChakraUI"
datePublished: Fri Dec 02 2022 05:29:14 GMT+0000 (Coordinated Universal Time)
cuid: clb62fzqx000308jh7bw1bts7
slug: build-a-modern-user-interface-with-chakraui
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1669959331151/o3RzzYFBL.png
tags: typescript, ui, chakra-ui

---

### Install and Setup Chakra UI in a React Project

```bash
npx create-next-app --ts
# or
yarn create next-app --typescript
```

```bash
yarn add @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^4
```

[AppProps](https://nextjs.org/docs/basic-features/typescript#custom-app) is a type that is given to us by Next.js and Typescript.

```javascript
import { AppProps } from 'next/app';

const App = () => {

};

export default App;
```

```javascript
const App = ({ Component, pageProps }: AppProps) => {
  return <Component {...pageProps} />
};
```

```javascript
import { ChakraProvider } from '@chakra-ui/react';

const App = ({ Component, pageProps }: AppProps) => {
  return 
    <ChakraProvider>
      <Component {...pageProps} />
    </ChakraProvider>
};
```

### Build a Layout with the Container, Flex and VStack Component in Chakra UI

[Figma](https://www.figma.com/) is a vector graphics editor and prototyping tool which is primarily web-based, with additional offline features enabled by desktop applications for macOS and Windows.

[Containers](https://chakra-ui.com/docs/layout/container) are used to constrain a content's width to the current breakpoint while keeping it fluid.

```javascript

import { Container } from '@chakra-ui/react';
```

```javascript
const IndexPage = () => (
  <Container>

  </Container>
)
```

[Style props](https://chakra-ui.com/docs/features/style-props) are a way to alter the style of a component by simply passing props to it.

The [sizes](https://chakra-ui.com/docs/theming/theme#sizes) key allows you to customize the global sizing of components you build for your project.

```javascript
const IndexPage = () => (
  <Container maxWidth="container.xl" padding={0}>

  </Container>
)
```

[Flex](https://chakra-ui.com/docs/layout/flex) is Box with `display: flex` and comes with helpful style shorthand. It renders a `div` element.

```javascript
import { Container, Flex } from '@chakra-ui/react'; 

const IndexPage = () => (
  <Container maxWidth="container.xl" padding={0}>
    <Flex h="100vh" py={20}>

    </Flex>
  </Container>
)
```

[VStack](https://chakra-ui.com/docs/layout/stack) is used to add spacing between elements in vertical direction only, and centers them.

```javascript
import { Container, Flex, VStack } from '@chakra-ui/react'; 

const IndexPage = () => (
  <Container maxWidth="container.xl" padding={0}>
    <Flex h="100vh" py={20}>
      <VStack
        w="full"
        h="full"
        p={10}
        spacing={10}
        alignItems="flex-start"
      ></VStack>
    </Flex>
  </Container>
)
```

```javascript
const IndexPage = () => (
  <Container maxWidth="container.xl" padding={0}>
    <Flex h="100vh" py={20}>
      <VStack
        w="full"
        h="full"
        p={10}
        spacing={10}
        alignItems="flex-start"
      ></VStack>
      <VStack
        w="full"
        h="full"
        p={10}
        spacing={10}
        alignItems="flex-start"
        bg="gray.50"
      ></VStack>
    </Flex>
  </Container>
)
```

### Build a 2-Column Form with the SimpleGrid, FormControl, and Input Component in Chakra UI

[Headings](https://chakra-ui.com/docs/typography/heading) are used for rendering headlines.

```javascript
import { VStack, Heading } from '@chakra-ui/react';

const Details = () => {
  return (
    <VStack w="full" h="full" p={10} spacing={10} alignItems="flex-start">
      <Heading size="2xl">Your details</Heading>
    </VStack>
  )
}
```

[Text](https://chakra-ui.com/docs/typography/text) component is the used to render text and paragraphs within an interface.

```javascript
import { VStack, Heading, Text } from '@chakra-ui/react';

<Heading size="2xl">Your details</Heading>
<Text>If you already have an account, click here to log in.</Text>
```

Overriding a parent's styling is quite easy in Chakra UI. To override the parent's `spacing`, we can wrap our `Heading` and our `Text` in another `VStack` and set `spacing={3}`. In existing react components, trying to override them may force you to restyle your parent components along with their internals.

### Create a Dark Mode Switcher in Chakra UI

[useColorMode](https://chakra-ui.com/docs/features/color-mode#usecolormode) is a React hook that gives you access to the current color mode, and a function to toggle the color mode.

```javascript
const Cart = () => {
  const { toggleColorMode } = useColorMode();
  ...
  <Button onClick={toggleColorMode} variant="link" colorScheme="black">
    try changing the theme.
  </Button>
}
```

[useColorModeValue](https://chakra-ui.com/docs/features/color-mode#usecolormodevalue) is a React hook used to change any value or style based on the color mode. It takes 2 arguments: the value in light mode, and the value in dark mode.

```javascript
const bgColor = useColorModeValue("gray.50", "whiteAlpha.50")

<VStack
  w="full"
  h="full"
  p={10}
  spacing={6}
  align="Flex-start"
  bg={bgColor}
>
```

```javascript
const secondaryTextColor = useColorModeValue("gray.600", "gray.400")

<Text color="gray.600"> 
// turns into
<Text color={secondaryTextColor}>
```

```javascript
<VStack spacing={3} alignItems="flex-start">
  <Heading size="2xl">Your details</Heading>
  <Text>If you already have an account, click here to log in.</Text>
</VStack>
```

[Grid](https://chakra-ui.com/docs/layout/grid) is a primitive useful for grid layouts. Grid is `Box` with `display: grid` and it comes with helpful style shorthand. It renders a `div` element.

```javascript
import { VStack, Heading, Text, SimpleGrid, GridItem } from '@chakra-ui/react';

<SimpleGrid columns={2} columnGap={3} rowGap={6} w="full">

</SimpleGrid>
```

[FormControl](https://chakra-ui.com/docs/form/form-control) provides context such as `isInvalid`, `isDisabled`, and `isRequired` to form elements. Since our `SimpleGrid` is two columns, we can make one of our inputs only take up one of those columns using `colSpan={1}`.

```tsx
import { ..., FormControl, FormLabel, Input } from '@chakra-ui/react';

<SimpleGrid columns={2} columnGap={3} rowGap={6} w="full">
  <GridItem colSpan={1}>
    <FormControl>
      <FormLabel>First Name</FormLabel>
      <Input placeholder="John"/>
    </FormControl>
  </GridItem>
</SimpleGrid>
```

```tsx
<GridItem colSpan={1}>
  <FormControl>
    <FormLabel>Last Name</FormLabel>
    <Input placeholder="Doe"/>
  </FormControl>
</GridItem>
```

```tsx
<GridItem colSpan={2}>
  <FormControl>
    <FormLabel>Address</FormLabel>
    <Input placeholder="Blvd. Broken Dreams 21"/>
  </FormControl>
</GridItem>
```

```tsx
<GridItem colSpan={1}>
  <FormControl>
    <FormLabel>City</FormLabel>
    <Input placeholder="San Francisco"/>
  </FormControl>
</GridItem>
```

[Select](https://chakra-ui.com/docs/form/select) component is a component that allows users pick a value from predefined options. Ideally, it should be used when there are more than 5 options, otherwise you might consider using a radio group instead.

```tsx
import { ..., Select } from '@chakra-ui/react';

<GridItem colSpan={1}>
  <FormControl>
    <FormLabel>City</FormLabel>
    <Select>
      <option value="usa">United States of America</option>
      <option value="uae">United Arab Emirates</option>
      <option value="nmk">North Macedonia</option>
      <option value="de">Germany</option>
    </Select>
  </FormControl>
</GridItem>
```

The [Checkbox](https://chakra-ui.com/docs/form/checkbox) component is used in forms when a user needs to select multiple values from several options.

```tsx
import { ..., Checkbox, Button } from '@chakra-ui/react';

<GridItem colSpan={2}>
  <CheckBox defaultChecked>Ship to billing address.</CheckBox>
</GridItem>
```

```tsx
<GridItem colSpan={2}>
  <Button size="lb" w="full">
    Place Order
  </Button>
</GridItem>
```

### Implement Responsive Design in Chakra UI

All style props accept arrays as values for mobile-first responsive styles. This is known as [The Array syntax](https://chakra-ui.com/docs/features/responsive-styles#the-array-syntax).

This means that in our array, we get 0 from 0 to 479 pixels, 10 from 480 pixels until 767 pixels, and then 20 from 768 pixels and up.

```javascript
<Flex h="100vh" py={[0, 10, 20]}>
  <Details />
  <Cart />
</Flex>
```

By using [Responsive Direction](https://chakra-ui.com/docs/layout/stack#responsive-direction), we can pass responsive values to our `Flex` component to change the stack direction and/or the spacing between our elements.

```javascript
<Flex 
  h="100vh" 
  py={[0, 10, 20]}
  direction={{ base: 'column-reverse', md: 'row' }}
>
  <Details />
  <Cart />
</Flex>
```

```javascript
<Flex 
  h={{ base: 'auto', md: '100vh' }} 
  py={[0, 10, 20]}
  direction={{ base: 'column-reverse', md: 'row' }}
>
  <Details />
  <Cart />
</Flex>
```

The [useBreakpointValue](https://chakra-ui.com/docs/hooks/use-breakpoint-value) is a custom hook which returns the value for the current breakpoint from the provided responsive values object. This hook also responds to the window resizing and returning the appropriate value for the new window size.

```tsx
import { ..., useBreakpointValue } from '@chakra-ui/react';

const colSpan = useBreakpointValue({ base: 2, md: 1 });
```

```javascript
<GridItem colSpan={1}>
// All of them will now be....
<GridItem colSpan={colSpan}>
```

### Define Custom Colors and Fonts in Chakra UI

By default, all Chakra components inherit values from the default theme. In some scenarios, you might need to customize the theme tokens to match your design requirements. This is where [extendTheme](https://chakra-ui.com/docs/theming/customize-theme#theme-extension-withdefaultcolorscheme) comes in to play.

```javascript
import { extendTheme} from '@chakra-ui/rect';

const theme = extendTheme({});

export default theme;
```

```javascript
import theme from '../src/theme';

const App = ({ Component, pageProps }: AppProps) => {
  return ( 
    <ChakraProvider theme={theme}>
      <Component {...pageProps}>
    </ChakraProvider>
  )
}
```

```javascript
const theme = extendTheme({
  fonts: {
    heading: 'Montserrat',
    body: 'Inter',
  }
});
```

```javascript
import { ..., theme as base } from '@chakra-ui/rect';

const theme = extendTheme({
  fonts: {
    heading: `Montserrat, ${base.fonts?.heading}`,
    body: `Inter, ${base.fonts?.body}`,
  }
});
```

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Montserrat:wght@700&display=swap');
```

```javascript
colors: {
  brand: {
    50: '#f5fee5',
    100: '#e1fbb2',
    200: '#cdf781',
    300: '#b8ee56',
    400: '#a2e032',
    500: '#8ac919',
    600: '#71ab09',
    700: '#578602',
    800: '#3c5e00',
    900: '#203300',
  },
},
```

```javascript
<Button colorScheme="brand" size="lg" w="full">
  Place Order
</Button>
```

### Use Theme Extensions in Chakra UI

[withDefaultColorScheme](https://chakra-ui.com/docs/theming/customize-theme#theme-extension-withdefaultcolorscheme) does exactly what is in the name, sets a default color scheme to all of your components. It takes an object where you can set your `colorScheme` to a color, and you can set it to specifically focus on certain components such as `Button` or `Badge`.

Most components in Chakra UI have default styling. [Variants](https://chakra-ui.com/docs/theming/component-style#base-styles-and-modifier-styles) are one way we can override those default settings and set certain components to have a different visual styles. [withDefaultVariant](https://chakra-ui.com/docs/theming/customize-theme#theme-extension-withdefaultvariant) is how we handle our variants.

```javascript
withDefaultColorScheme({
  colorScheme: 'brand',
  components: ['Checkbox'],
})
withDefaultVariant({
  variant: 'filled',
  components: ['Input', 'Select'],
})
```

### Override the Built-in Component's Styles in Chakra UI

Continuing on how we override the default styling of our components, without using any theme extensions, we are going to use the [basic API](https://chakra-ui.com/docs/theming/component-style#styling-single-part-components) for this.

```javascript
export default {
  // Styles for the base style
  baseStyle: {},
  // Styles for the size variations
  sizes: {},
  // Styles for the visual style variations
  variants: {},
  // The default `size` or `variant` values
  defaultProps: {},
}
```

```javascript
components: {
  Input: {
    sizes: {
      md: {
        field: {
          borderRadius: 'none',
        }
      }
    }
  },
},
```

```javascript
variants: {
  filled: {
    field: {
      _focus: {
        borderColor: 'brand.500',
      },
    },
  },
},
```

```javascript
Checkbox: {
  baseStyle: {
    control: {
      borderRadius: 'none',
      _focus: {
        ring: 2, 
        ringColor: 'brand.500',
      }
    }
  }
}
```

### Create Custom Variants in Chakra UI

Just like we edited the styling of our components in the last two lessons, we are going to do the same to our variants to create our own custom variants. Just make sure to use the `variant` prop on your components in your details.tsx file and set it to what you named your variant, in our case `primary`.

```javascript
Button: {
  variants: {
    primary: {
      rounded: 'none',
    },
  },
},
```

```javascript
Button: {
  variants: {
    primary: (props) => ({
      rounded: 'none',
      ...brandRing,
      backgroundColor: mode('brand.500', 'brand.200')(props),
    }),
  },
},
```

```javascript
Button: {
  variants: {
    primary: (props) => ({
      rounded: 'none',
      ...brandRing,
      backgroundColor: mode('brand.500', 'brand.200')(props),

      _hover: {
        backgroundColor: mode('brand.600', 'brand.300')(props),
      },

      _active: {
        backgroundColor: mode('brand.700', 'brand.400')(props),
      },
    }),
  },
},
```

```javascript
Button: {
  variants: {
    primary: (props) => ({
      rounded: 'none',
      ...brandRing,
      color: mode('white', 'gray.800')(props),
      backgroundColor: mode('brand.500', 'brand.200')(props),

      _hover: {
        backgroundColor: mode('brand.600', 'brand.300')(props),
      },

      _active: {
        backgroundColor: mode('brand.700', 'brand.400')(props),
      },
    }),
  },
},
```