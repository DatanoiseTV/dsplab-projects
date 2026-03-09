# DSPLab Community Projects

This repository hosts community-contributed Vult DSP programs for [DSPLab](https://github.com/DatanoiseTV/dsplab).

## Structure

```
<author>/
  my_preset.vult
  another_preset.vult
community/
  shared_patch.vult
```

## Contributing

1. Fork this repo
2. Create a folder with your GitHub username
3. Add your `.vult` files — one file per preset
4. Open a pull request

## Guidelines

- One synthesizer or effect per file
- Include a short comment at the top describing what it does
- Files must compile without errors in DSPLab
- All mandatory handlers required: `noteOn`, `noteOff`, `controlChange`, `default`
