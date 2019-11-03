---
title: "Fibonacci C++ Template-Based"
date: 2019-11-02
draft: true
category: "C++"
tags: ["C++", "C++ Templates"]
---

Fibonacci on C++ templates,

<!--more-->


{{< highlight CPP "linenos=inline" >}}
#include <array>
#include <cstdint>
#include <iostream>
#include <stdexcept>

using namespace std;

#if 0 // Not in C++11 // make_index_sequence

template <std::size_t...> struct index_sequence {};

template <std::size_t N, std::size_t... Is>
struct make_index_sequence : make_index_sequence<N - 1, N - 1, Is...> {};

template <std::size_t... Is>
struct make_index_sequence<0u, Is...> : index_sequence<Is...> {};

#endif // make_index_sequence

namespace detail
{

	template <std::size_t N>
	struct Fibo :
		std::integral_constant<size_t, Fibo<N - 1>::value + Fibo<N - 2>::value>
	{
		static_assert(Fibo<N - 1>::value + Fibo<N - 2>::value >= Fibo<N - 1>::value,
			"overflow");
	};

	template <> struct Fibo<0u> : std::integral_constant<size_t, 0u> {};
	template <> struct Fibo<1u> : std::integral_constant<size_t, 1u> {};

	template <std::size_t ... Is>
	constexpr std::size_t fibo(std::size_t n, index_sequence<Is...>)
	{
		return const_cast<const std::array<std::size_t, sizeof...(Is)>&&>(
			std::array<std::size_t, sizeof...(Is)>{ {Fibo<Is>::value...}})[n];
	}

	template <std::size_t N>
	constexpr std::size_t fibo(std::size_t n)
	{
		return n < N ? fibo(n, make_index_sequence<N>()) : throw std::runtime_error("out of bound");
	}
} // namespace detail

constexpr std::size_t fibo(std::size_t n)
{
	// 48u is the highest
	return detail::fibo<48u>(n);
}

int main()
{
	std::cout << fibo(42) << std::endl;
	std::size_t input;
	std::cin >> input;
	std::cout << fibo(input) << std::endl;

	return 0;
}
{{< / highlight >}}