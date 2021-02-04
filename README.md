# native-x-form

[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

This component helps you add and manage form in react native

## Install

### Yarn

```sh
yarn add native-x-form
```

### NPM

```sh
npm install native-x-form
```

## Usage

```tsx
import { Form, FormItem, isEmpty, isInvalidEmail } from 'native-x-form'

interface FormData {
  email: string
  password: string
}

const state: FormData = {
  email: '',
  password: '',
}

function MyComponent() {
  const onSubmit = useCallback(
    async ({ state: { email, password }, isValid }: { state: FormData; isValid: boolean }) => {
      if (!isValid) {
        return
      }
      // your logic
    },
    [],
  )

  return (
    <Form<FormData> state={state} onSubmit={onSubmit}>
      <FormItem name='email' validators={[isEmpty('Email is required'), isInvalidEmail()]}>
        <TextInput />
      </FormItem>
    </Form>
  )
}
```

Any component can be placed inside `FormItem` as long as the props are extended from `FormChildProp`

```tsx
export type AcceptableFormValue = string | boolean | number

type FormChildProp<T extends AcceptableFormValue> = {
  value?: T
  onChange?: (value: T) => void
  onChangeText?: (value: T) => void
  onBlur?: () => void
  error?: string | Error | null
  isLoading?: boolean
}
```

Sample implementation:

```tsx
import { FormChildProps } from 'native-x-form'

interface InputProps extends FormChildProps<string> {
  ...
}

function Input(props: InputProps) {
  const {
    value,
    onChange,
    onChangeText,
    onBlur,
    error,
    isLoading
  } = props
  return (
    ...
  )
}
```

## API - Form

| Property                                                          | Default Value | Usage                           |
| ----------------------------------------------------------------- | ------------- | ------------------------------- |
| state?: T                                                         | any           | State of the form               |
| children?: ReactNode[]                                            |               | Content of the form             |
| onSubmit?: (props: { state: T, isValid: boolean}) => Promise<any> |               | Handler for submitting the form |

## API - Form-Item

| Property                   | Default Value | Usage                                                |
| -------------------------- | ------------- | ---------------------------------------------------- |
| name: string               | any           | Name of the item in `state`. This input is mandatory |
| children?: ReactNode[]     |               | Content of the form                                  |
| validators: Validator<T>[] |               | An array of validators                               |

## Validators

You can build a custom validator function by implementing `Validator<T>` type

```tsx
export type Validator<T> = (input: T) => string | undefined
```

The return value of `Validator` function must be the error message.

Few validator function creators are provided with this module.

```tsx
isEmpty(errorMessage: string): Validator<T>
isInvalidEmail(errorMessage: string): Validator<T>
isNonAlphaNumeric(errorMessage: string): Validator<T>
```

## Automatic Release

Here is an example of the release type that will be done based on a commit messages:

| Commit message      | Release type          |
| ------------------- | --------------------- |
| fix: [comment]      | Patch Release         |
| feat: [comment]     | Minor Feature Release |
| perf: [comment]     | Major Feature Release |
| doc: [comment]      | No Release            |
| refactor: [comment] | No Release            |
| chore: [comment]    | No Release            |
