import React, { useState, useEffect, useCallback } from 'react';
import axios from 'axios';

const useFetch = (url, fn) => {
	const controller = new AbortController();

	const [data, setData] = useState(null);
	const [error, setError] = useState(null);
	const [loading, setLoading] = useState(false);
	const [path, setPath] = useState(url);

	const fetchData = useCallback(async () => {
		try {
			setLoading(true);
			const response = await axios.get(path, {
				signal: controller.signal,
			});
			console.log('response', response);
			setData(response.data);
			fn?.(response.data);
			setLoading(false);
		} catch (error) {
			console.log('error', error);
			setLoading(false);
			setError(error);
		}
	}, [path, fn]);

	useEffect(() => {
		if (path) {
			fetchData();
		}
		return () => {
			controller.abort();
		};
	}, [path]);

	return {
		data,
		error,
		loading,
		setPath,
	};
};

const ChildComponent = () => {
	const { loading, error, setPath, data } = useFetch(
		'https://api.kanye.rest',
		(response) => {
			console.log(response);
		}
	);

	console.log({ loading, error, setPath, data });

	return (
		<div>
			<h1>ChildComponent</h1>
			<p>Click among buttons to switch url and fetch new data</p>
			<p>
				{loading
					? 'loading...'
					: data
					? JSON.stringify(data)
					: 'Please try again...'}
			</p>
			<button onClick={() => setPath('https://api.kanye.rest')}>
				Kanye Quotes
			</button>
			<button onClick={() => setPath('https://catfact.ninja/fact')}>
				Cat facts
			</button>
			<button onClick={() => setPath('https://www.boredapi.com/api/activity')}>
				What to do
			</button>
			<button
				onClick={() => setPath('https://dog.ceo/api/breeds/image/random')}
			>
				Dogs
			</button>
		</div>
	);
};

export default function App() {
	const [show, setShow] = useState(false);

	return (
		<div className="App">
			<h1>Hello Savannah tech</h1>
			<h2>
				Click toggle to mount or unmount a child component that utilises the
				useFetch hook
			</h2>
			<button onClick={() => setShow((value) => !value)}>Toggle</button>
			{show && <ChildComponent />}
		</div>
	);
}
