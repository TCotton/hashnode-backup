## Mocking the Web Audio API in Jest

This is code for mocking the Web Audio API in Jest that may be of use to you. The methods mocked and the number of times called will vary depending on your needs.

This code was used in a side-project that  [allows users to view, convert and transcode video media for Twitter, Facebook or Linkedin](https://convertvideo.forsocialmedia.link/) .

    describe('useAudioContext', () => {

    const mockConnect = jest.fn();
    const mockcreateMediaElementSource = jest.fn(() => {
        return {
            connect: mockConnect
        }
    });
    const mockgetByteFrequencyData = jest.fn();
    const mockcreateAnalyser = jest.fn(() => {
        return {
            connect: mockConnect,
            frequencyBinCount: [0, 1, 2],
            getByteFrequencyData: mockgetByteFrequencyData,
        }
    });
    const mockcreateOscillator = jest.fn(() => {
        return {
            channelCount: 2
        }
    });
    const mockChannelSplitterConnect = jest.fn(n => n);
    const mockcreateChannelSplitter = jest.fn(() => {
        return {
            connect: mockChannelSplitterConnect
        }
    });
    const mockaudioContext = jest.fn(() => {
        return {
            createAnalyser: mockcreateAnalyser,
            createMediaElementSource: mockcreateMediaElementSource,
            createOscillator: mockcreateOscillator,
            createChannelSplitter: mockcreateChannelSplitter,
        }
    });


    beforeEach(() => {
        window.AudioContext = mockaudioContext;
    });

    it('should return false for default value', () => {
        setUp({current: 'current'});

        expect(mockcreateAnalyser).toBeCalledTimes(3);
        expect(mockcreateMediaElementSource).toBeCalledTimes(1);
        expect(mockConnect).toBeCalledTimes(3);
        expect(mockcreateOscillator).toBeCalledTimes(1);
        expect(mockcreateChannelSplitter).toBeCalledTimes(1);
        expect(mockChannelSplitterConnect).toBeCalledTimes(2);
        expect(mockgetByteFrequencyData).toBeCalledTimes(4);
    });
    })

