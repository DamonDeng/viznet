# Viznet - A Neural Network visualization toolbox

## To Run a Sample
```bash
    $ python -m viznet.tests.test_netbasic
```

I created a theme for neural network plotting following [Neural Network Zoo Page](http://www.asimovinstitute.org/neural-network-zoo/),
here is a table of node species:

![theme_list](docs/images/theme_list.png)


```python
NODE_THEME_DICT = {
        'basic': (NONE, 'none'),
        'backfed': (YELLOW, 'circle'),
        'input': (YELLOW, 'none'),
        'noisy_input': (YELLOW, 'triangle'),
        'hidden': (GREEN, 'none'),
        'probablistic_hidden': (GREEN, 'circle'),
        'spiking_hidden': (GREEN, 'triangle'),
        'output': (RED, 'none'),
        'match_input_output': (RED, 'circle'),
        'recurrent': (BLUE, 'none'),
        'memory': (BLUE, 'circle'),
        'different_memory': (BLUE, 'triangle'),
        'kernel': (VIOLET, 'none'),
        'convolution': (VIOLET, 'circle'),
        'pooling': (VIOLET, 'circle'),
        }
```

## Example A: Restricted Boltzmann Machine
![theme_list](docs/images/rbm.png)
## Example B: Feed Forward Neural Network
![theme_list](docs/images/feed_forward.png)
## Example C: Convolutional Neural Network on quivalence of RBM
![theme_list](docs/images/conv_rbm.png)
The code looks like
```python
from ..netbasic import DynamicShow, NNPlot

def draw_conv_rbm(ax, num_node_visible, num_node_hidden):
    '''CNN equivalance to RBM'''
    handler = NNPlot(ax)
    # visible layers
    handler.add_node_sequence(
        num_node_visible, '\sigma^z', 0, kind='input', radius=0.3)

    # hidden layers
    handler.add_node_sequence(num_node_hidden, 'h',
                              1.5, kind='convolution', radius=0.3)

    # nonlinear layers
    handler.add_node_sequence(num_node_hidden, 'nonlinear',
                              2.3, kind='basic', radius=0.15, show_name=False)
    handler.text_node_sequence('nonlinear', text_list=[r'$\log 2\cosh$'],
                               offset=(-0.6,-0.4))

    # sum, we do not show_name because name automatically generated
    # will be r"$+_1$" (with subscript)
    handler.add_node_sequence(1, '+',
                              3.1, kind='basic', radius=0.15, show_name=False)
    handler.text_node_sequence('+', text_list=[r'$+$'])

    # output
    handler.add_node_sequence(1, r'\psi',
                              3.9, kind='output', radius=0.3, show_name=False)
    handler.text_node_sequence(r'\psi', text_list=[r'$\psi$'], offset=(0,0.))

    # connect them
    handler.connect_layers('\sigma^z', 'h', directed=True)
    handler.connect_layers('h', 'nonlinear', directed=True, one2one=True)
    handler.connect_layers('nonlinear', '+', directed=False)
    handler.connect_layers('+', r'\psi', directed=True)

def test_conv_rbm():
    with DynamicShow((6, 6), '_conv_rbm.png') as d:
        draw_conv_rbm(d.ax, 5, 4)

if __name__ == '__main__':
    test_conv_rbm()
```